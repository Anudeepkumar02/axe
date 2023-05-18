pipeline {
    agent any

    stages {
        stage('git Checkout') {
            steps {
                git 'https://github.com/Anudeepkumar02/axe.git'
            }
        }
        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Package Download') {
            steps {
                sh 'aws s3 cp target/*.war s3://warfileswebproject/axe/axe.war'
                sh 'aws s3 cp target/*.war s3://warfileswebproject/axe/axe-$BUILD_NUMBER.war'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t axe:V-$BUILD_NUMBER .'
            }
        }
        stage('Push Docker Image to Dockerhub') {
            steps {
                sh 'docker login -u 2222s -p Anu@#deep123'
                sh 'docker tag axe:V-$BUILD_NUMBER 2222s/axe:V-$BUILD_NUMBER'
                sh 'docker push 2222s/axe:V-$BUILD_NUMBER'
            }
        }
     stage('Deploy Docker image to containers') {
         steps {
             ansiblePlaybook credentialsId: 'ansible_cred', disableHostKeyChecking: true, inventory: 'uat.in', playbook: 'Docker-deploy.yml'
            }
        }
    }
}
