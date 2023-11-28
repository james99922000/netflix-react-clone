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
                    docker.build("141806876594.dkr.ecr.us-east-1.amazonaws.com/netflixe-oct:v1")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 141806876594.dkr.ecr.us-east-1.amazonaws.com/netflixe-oct:v1'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('141806876594.dkr.ecr.us-east-1.amazonaws.com/netflixe-oct:v1', 'ecr:us-east-1:jamesC-acr') {
                    // build image
                    def myImage = docker.build("141806876594.dkr.ecr.us-east-1.amazonaws.com/netflixe-oct:v1")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
