pipeline {
    agent any
    tools{
        maven 'maven_3_5_0'
        dockerTool 'docker'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/keerthi-ns/jenkins-kubernetes-automation']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    def dockerHome = tool 'docker'
                    sh 'docker build -t keerthins123/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u keerthins123 -p ${dockerhubpwd}'

}
                   sh 'docker push keerthins123/devops-integration'
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
