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
        stage('Approval to Deploy') {
            steps {
                script {
                    def userInput = input(
                        id: 'DeployApproval', message: 'Approve deployment to LIVE your Hotel website?', parameters: [
                            [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'Tick to approve deployment', name: 'Proceed']
                        ]
                    )
                    if (!userInput) {
                        error("❌ Deployment aborted by user.")
                    }
                }
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
            script {
                httpRequest httpMode: 'POST',
                    url: "${env.slack_webhook}",
                    contentType: 'APPLICATION_JSON',
                    requestBody: '{"text": "✅ Build succeeded! Your website is live."}'
            }
        }

        failure {
            script {
                httpRequest httpMode: 'POST',
                    url: "${env.slack_webhook}",
                    contentType: 'APPLICATION_JSON',
                    requestBody: '{"text": "❌ Build failed! Check Jenkins for details."}'
            }
        }
    }
}

