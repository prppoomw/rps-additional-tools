pipeline {
    agent any

    parameters {
        string(name: 'EC2_HOST', description: 'Target server to deploy docker image e.g. ec2-user@your-ec2-public-ip')
    }

    environment {
        IMAGE_TAG = "${env.BUILD_ID}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Deploy to EC2') {
            steps {
                sshagent(credentials: ['ec2-ssh-credentials']) {
                    script {
                        def remoteCommand = """
                            rm -rf rps-additional-tools &&
                            git clone https://github.com/prppoomw/rps-additional-tools.git &&
                            cd mysql-kafka-redis &&
                            docker-compose up -d
                        """

                        sh """
                            ssh -o StrictHostKeyChecking=no ${params.EC2_HOST} '${remoteCommand}'
                        """
                    }
                }
            }
        }
    }
}