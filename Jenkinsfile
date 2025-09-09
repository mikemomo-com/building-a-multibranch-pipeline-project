pipeline {
    agent {
        kubernetes {
            yaml '''
                spec:
                    containers:
                    - name: maven
                      image: maven:3.9.9-eclipse-temurin-17
                      command: ["tail", "-f", "/dev/null"]
                    - name: node
                    image: node:20-alpine
                    command: ['cat']
                    tty: true
            '''
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                container('node') {
                    sh 'npm install'
                }
            }
        }
        stage('Test') {
            steps {
                container('node') {
                    sh './jenkins/scripts/test.sh'
                }
            }
        }
    }
}