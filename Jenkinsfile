pipeline {
    agent any
    
    tools {
        maven 'M2_HOME'
    }
    environment {
        AWS_ACCESS_KEY_ID = 'AKIAR3EPJ2LQYBJEX535'
        AWS_SECRET_KEY = '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'
        }

    stages {
        stage('Checkout') {
            steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/prashantsuragimath/devops-certification-project1.git']]])
            }
        }
        stage('Compile') {
            steps {
            sh 'mvn clean package'
            }
        }  
        stage('Docker Build') {
            steps {
                sh 'docker build -t pkcsmath/project1 .'
            }
        }
        stage('Push Docker Image to Dockerhub') {
      steps{
        script {
            withDockerRegistry([ credentialsId: "docker-hub", url: "https://registry.hub.docker.com" ]){
                sh 'docker login -u cbabu85 -p ${Prashant@1989}'
            }
            {
            dockerImage.push()
            }
        }
      }
    }
         stage('Terraform init') {
             steps {
                 sh 'terraform init'
             }
         }
         stage('Terraform Apply') {
             steps {
                 sh 'terraform apply --auto-approve'
                 sleep 20
             }
         }
        stage('Docker Deploy using Ansible') {
             steps {
                 ansiblePlaybook credentialsId: 'terraform-docker', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: 'deploy-docker.yml'
             }
        }
     }
}
