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
            script {
                def payload = [
                    text: ":white_check_mark: *SUCCESS:* Website deployed successfully on port *9090*."
                ]
                httpRequest httpMode: 'POST',
                            contentType: 'APPLICATION_JSON',
                            requestBody: groovy.json.JsonOutput.toJson(payload),
                            url: slack_webhook
            }
        }

        failure {
            script {
                def payload = [
                    text: ":x: *FAILURE:* Website deployment failed. Please check the Jenkins logs for errors."
                ]
                httpRequest httpMode: 'POST',
                            contentType: 'APPLICATION_JSON',
                            requestBody: groovy.json.JsonOutput.toJson(payload),
                            url: slack_webhook
            }
        }
    }
}

