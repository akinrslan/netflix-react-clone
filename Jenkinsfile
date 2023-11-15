pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages{
        // NPM dependencies
        stage('pull npm dependencies') {
            steps {
                sh 'npm install'
            }
        }
       stage('build Docker Image') {
            steps {
                script {
                    // build image
                    docker.build("716819891591.dkr.ecr.us-west-2.amazonaws.com/netflix-app:latest")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 716819891591.dkr.ecr.us-west-2.amazonaws.com/netflix-app:latest'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https:716819891591.dkr.ecr.us-west-2.amazonaws.com/netflix-app:latest', 'ecr:us-west-2:akinino-ecr') {
                    // build image
                    def myImage = docker.build("716819891591.dkr.ecr.us-west-2.amazonaws.com/netflix-app:latest")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
