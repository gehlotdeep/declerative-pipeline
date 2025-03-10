pipeline {
    agent any
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/deepak']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/gehlotdeep/declerative-pipeline.git/']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t deepakgehlot/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubPass")]) {
                   sh 'docker login -u deepakgehlot -p ${dockerhubPass}'

}
                   sh 'docker push deepakgehlot/devops-integration'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwd')
                }
            }
        }
    }
}