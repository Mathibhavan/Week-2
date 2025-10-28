pipeline{
    environment {
        EC2_USER='ec2-user'
        EC2_HOST="34.201.242.131"
        SSH_KEY= Downloads/mathi_key.pem
        REPO_URL='https://github.com/Mathibhavan/Week-2.git'
        BRANCH='master'
        DEPLOY_DIR='VAR/WWW/html'
    }
    stages{
        stage('Checkout')
        steps{
            git url: ${REPO_URL}
            git checkout ${BRANCH}
        }
        stage('Deploy to EC2 instance')
        steps{
            sh """
                ssh -i ${SSH_KEY_PATH} -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} 'rm -rf ${DEPLOY_DIR}/*'
                scp -i ${SSH_KEY_PATH} -o StrictHostKeyChecking=no -r * ${EC2_USER}@${EC2_HOST}:${DEPLOY_DIR}
                """
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
}