pipeline {
    agent any
    tools{
        maven 'maven'
    }
    
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/kannan50devops/devops.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t kannanbe50/kubernetes .'
                }
            }
        }
        stage('Push image to hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'Hornet@900')]) {
                    sh 'docker login -u kannanbe50 -p ${dockerhubpwd}'
                        
                    }
                    sh 'docker push kannanbe50/kubernetes'
                }
            }
        }
        stage('Deploy to K8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'kubeconfig')
                }
            }
        }
    
    }    
}
