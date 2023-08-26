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
                    docker.build("481195358488.dkr.ecr.us-east-1.amazonaws.com/netflix-app:v1.0.0.RELEASE")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 481195358488.dkr.ecr.us-east-1.amazonaws.com/netflix-app:v1.0.0.RELEASE'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://481195358488.dkr.ecr.us-east-1.amazonaws.com/netflix-app', 'ecr:us-east-1:taijo-ecr') {
                    // build image
                    def myImage = docker.build("481195358488.dkr.ecr.us-east-1.amazonaws.com/netflix-app:v1.0.0.RELEASE")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
