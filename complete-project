pipeline{
    agent any
    tools{
        maven 'Maven'
    }
    options {
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '30', numToKeepStr: '1')
    }
    environment{
        dockerhub_cred = credentials('docker-cred')
    }
    stages{
        stage('Checkout stage'){
            steps{
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/DevopsWorking/addressbook.git']])
            }
        }
        stage('Maven Build'){
            steps{
                sh 'mvn package'
            }
        }
        stage('docker build'){
            steps{
                sh "docker build -t tampoohoonm/project-docker:${BUILD_NUMBER} ."
            }
        }
        stage('dockerhub push'){
            steps{
                sh "echo $dockerhub_cred_PSW | docker login -u $dockerhub_cred_USR --password-stdin"
                sh "docker push tampoohoonm/project-docker:${BUILD_NUMBER}"
            }
        }
        stage('Docker run'){
            steps{
                sh "docker run -d -p 80:8080 --name addressbook tampoohoonm/project-docker:${BUILD_NUMBER}"
            }
        }
    }
    post {
        always {
            echo "job is completed"
        }
        success {
            echo "It is a success"
        }
        failure {
            echo "It has failed"
         }
    }
}
