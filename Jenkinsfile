pipeline{
    agent any
    environment {
        EC2_USER='ec2-user'
        EC2_HOST="44.201.254.15"
        //SSH_KEY_PATH='C:/ProgramData/Jenkins/.jenkins/keys/mathi_key.pem'
        REPO_URL='https://github.com/Mathibhavan/Week-2.git'
        BRANCH='master'
        DEPLOY_DIR='var/www/html'
    }
    stages{
        stage('Checkout'){
            steps{
            git branch: "${BRANCH}" , url: "${REPO_URL}"
            }
        }
        stage('Deploy to EC2 instance') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ec2_ssh_key', keyFileVariable: 'SSH_KEY_PATH')]) {
                    sh """
                        chmod 600 ${SSH_KEY_PATH}
                        ssh -i ${SSH_KEY_PATH} -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} 'rm -rf ${DEPLOY_DIR}/*'
                        scp -i ${SSH_KEY_PATH} -o StrictHostKeyChecking=no -r * ${EC2_USER}@${EC2_HOST}:${DEPLOY_DIR}
                    """
                }
            }
        }
    }

    }
    post{
        success{
            echo "Deployment Successful"
        }
        failure{
            echo "Deployment Failed"
        }
    }
