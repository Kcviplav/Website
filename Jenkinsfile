pipeline{
    agent any 

    stages{
        stage('Clone Code'){
            steps{
                echo 'CLoning the repository...'
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
                docker run -d -p 8080:80 --name my-website-container my-website-image 
                '''
            }
        }
    }
}

