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
        stage('Deliver for development') {
            when {
                branch 'development' 
            }
            steps {
                container('node') {
                    sh './jenkins/scripts/deliver-for-development.sh'
                    input message: 'Finished using the web site? (Click "Proceed" to continue)'
                    sh './jenkins/scripts/kill.sh'
                }
            }
        }
        stage('Deploy for production') {
            when {
                branch 'production'  
            }
            steps {
                container('node') {
                    sh './jenkins/scripts/deploy-for-production.sh'
                    input message: 'Finished using the web site? (Click "Proceed" to continue)'
                    sh './jenkins/scripts/kill.sh'
                }
            }
        }
    }
}