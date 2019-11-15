pipeline {
    agent any
    environment {
        //be sure to replace "willbla" with your own Docker Hub username
        DOCKER_IMAGE_NAME = "ravindrasingh6969/myapp"
    }
    stages {
        stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                    app.inside {
                        sh 'echo Hello, World!'
                    }
                }
            }
        }
            stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                         docker.withRegistry('https://006378141167.dkr.ecr.ap-south-1.amazonaws.com/ravindrasingh6969/myapp', 'ecr:ap-south-1:AKIAQC7BKWXXRP5ULRLB') 

                }
            }
        }
        stage('DeployToProduction') {
            when {
                branch 'master'
            }
            steps {
                input 'Deploy to Production?'
                milestone(1)    
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'test.yml',
                    enableConfigSubstitution: true
                )
            }
        }
    }
}
