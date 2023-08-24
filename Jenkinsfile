pipeline {
    agent any
    stages {
        stage('Compile and Clean') {
            steps {
                sh "mvn clean compile"
            }
        }
        stage('Build') {
            steps {
                sh "mvn package"
            }
        }
        stage('Test') {
            steps {
                sh "mvn test"
            }
        }
        stage('Code Coverage') {
            steps {
                sh "mvn jacoco:prepare-agent test jacoco:report"
            }
        }
        stage('SonarQube analysis') {
            steps {
                sh "echo 'Running SonarQube analysis'"
                sh "mvn sonar:sonar " +
                   "-Dsonar.projectKey=sonarqube " +
                   "-Dsonar.host.url=http://35.92.194.232:9000 \" +
                   "-Dsonar.login=sqp_c2a66a377d74ae653b1bd6e0ed5de78bbdd8cf38"
                sh "echo 'SonarQube analysis complete'"
            }
        }
        stage('Quality Gate Check') {
            steps {
                script {
                    def qualityGateStatus = sh(script: 'curl -s -u sqp_c2a66a377d74ae653b1bd6e0ed5de78bbdd8cf38: -X GET "http://54.183.208.209:9000/api/qualitygates/project_status?projectKey=sonarqube"', returnStdout: true).trim()
                    echo "Quality Gate Status: $qualityGateStatus"
                    if (qualityGateStatus.contains('ERROR') || qualityGateStatus.contains('WARN')) {
                        currentBuild.result = 'FAILURE'
                        error("Quality Gate not met")
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                sh "echo 'Performing deployment'"
                sh "echo 'Deployment complete'"
            }
        }
    }
}
