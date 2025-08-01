pipeline{
    agent any 
    environment {
        slack_webhook = 'https://hooks.slack.com/services/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' //WEBHOOK URL
    }

    stages{
        stage('Clone Code'){
            steps{
                echo 'Cloning the repository...'
                git url: 'https://github.com/Kcviplav/Website.git', branch: 'main'
            }
        }

        stage('Build Docker Image'){
            steps{
                echo 'Building Docker Image...'
                sh 'docker build -t my-website-image .'
            }
        }

        stage('Run Docker Container'){
            steps{
                echo 'Running Docker Container...'
                sh '''
                docker rm -f my-website-container || true
                docker run -d -p 9090:80 --name my-website-container my-website-image 
                '''
            }
        }
    }

    post {
        success {
            slackSend(
                channel: '#all-kc',
                message: '✅ Build succeeded!',
                tokenCredentialId: 'slack-webhook-2'
            )
        }

        failure {
            slackSend(
                channel: '#all-kc',
                message: '❌ Build failed!',
                tokenCredentialId: 'slack-webhook-2'
            )
        }
    }
}

