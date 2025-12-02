pipeline {
    agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: node
    image: node:18
    command:
    - cat
    tty: true
"""
        }
    }

    stages {
        stage('Build') {
            steps {
                container('node') {
                    sh 'npm --version'
                    sh 'node --version'
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
        when { branch 'development' }
        steps {
            container('node') {
                sh './jenkins/scripts/deliver-for-development.sh'
            }
            // input message: 'Finished using the web site? (Click "Proceed" to continue)'
            container('node') {
                sh './jenkins/scripts/kill.sh'
            }
        }
    }

    stage('Deploy for production') {
        when { branch 'production' }
        steps {
            container('node') {
                sh './jenkins/scripts/deploy-for-production.sh'
            }
            // input message: 'Finished using the web site? (Click "Proceed" to continue)'
            container('node') {
                sh './jenkins/scripts/kill.sh'
            }
        }
}

    }
}
