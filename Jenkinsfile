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
                script {
                    echo 'Running SonarQube analysis'
                    sh "mvn sonar:sonar " +
                       "-Dsonar.projectKey=sample " +
                       "-Dsonar.host.url=http://18.236.230.96:9000 " +
                       "-Dsonar.login=sqp_cb33f2c5dbfafe530c5ebf9288afb5370ed4d702
"
                    echo 'SonarQube analysis complete'
                }
            }
        }
        stage('Quality Gate Check') {
            steps {
                script {
                    def qualityGateStatus = sh(script: 'curl -s -u sqp_cb33f2c5dbfafe530c5ebf9288afb5370ed4d702
: -X GET "http://18.236.230.96:9000/api/qualitygates/project_status?projectKey=sample"', returnStdout: true).trim()
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

