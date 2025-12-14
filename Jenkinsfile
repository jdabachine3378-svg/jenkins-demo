pipeline {
    agent any

    tools {
        maven 'maven'
    }

    stages {
        stage('Git Clone') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/master']],
                    userRemoteConfigs: [[url: 'https://github.com/jdabachine3378-svg/jenkins-demo.git']]
                ])
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean install'
            }
        }

        stage('Create Docker Image') {
            steps {
                bat 'docker build -t myuser/pos .'
            }
        }

        stage('Run') {
            steps {
                bat 'docker rm -f test-pos || exit 0'
                bat 'docker run --name test-pos -d -p 8585:8282 myuser/pos'
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

