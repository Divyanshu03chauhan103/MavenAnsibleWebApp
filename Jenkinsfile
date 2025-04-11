pipeline {
    agent any  // Use any available agent

    environment {
        LANG = 'en_US.UTF-8'
        LC_ALL = 'en_US.UTF-8'
    }

    tools {
        maven 'Maven'  // Ensure this matches the name configured in Jenkins
    }

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()  // Clean workspace to avoid old or corrupted files
            }
        }

        stage('Build') {
            steps {
                dir("${env.WORKSPACE}") {
                    sh 'mvn clean package'  // Run Maven build
                }
            }
        }

        stage('Archive') {
            steps {
                dir("${env.WORKSPACE}") {
                    archiveArtifacts artifacts: 'target/*.war', fingerprint: true
                }
            }
        }

        stage('Deploy') {
            steps {
                dir("${env.WORKSPACE}") {
                    sh 'ansible-playbook ansible/playbook.yml -i ansible/hosts.ini'
                }
            }
        }
    }
}
