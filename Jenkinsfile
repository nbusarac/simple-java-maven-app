pipeline {
    environment {
	def scannerHome = tool 'sonarScanner';
    }


    agent any

    stages {
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh '${scannerHome}/bin/sonar-runner -X'
                }
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
