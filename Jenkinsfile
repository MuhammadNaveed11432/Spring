pipeline{
    agent any
    
    environment {
        IMAGE_TAG = "v${BUILD_NUMBER}"
    }
    
    tools{
         maven 'MAVEN_HOME'
    }
    
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/NaveedAmanat/SpringDemo.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'sudo docker build -t naveed0004/spring:${BUILD_NUMBER} .'
                }
            }
        }
        stage('Push Iamge to Docker Hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'DOCKER_HUB_KEY', variable: 'DOCKER_HUB')]) {
                        sh 'echo login to docker hub'
                    }
                }
            }
        }
        stage('Trigger Manifest'){
            steps{
                build job: 'SpringPipelineArtifact', parameters: [string(name: 'IMAGE_TAG', value: env.BUILD_NUMBER)]
            }
        }
    }
}
