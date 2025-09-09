pipeline {
    agent {
        kubernetes {
            yaml '''
                spec:
                    containers:
                    - name: maven
                        image: maven:3.9.9-eclipse-temurin-17
                        command: ["sleep"]
                        args: ["99d"]
            '''
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
    }
}