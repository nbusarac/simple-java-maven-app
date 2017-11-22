pipeline {
    agent any

    stages {
	stage('SonarQube analysis') {
	    steps {
                def scannerHome = tool 'sonarScanner';
                withSonarQubeEnv('sonar') {
                    sh '${scannerHome}/bin/sonar-runner'
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
