pipeline {
    agent any
    tools {
        maven 'Maven 3.9.6'
    }
    
    stages {
        
        stage('git checkout') {
            steps {
                git credentialsId: 'Github-Credentials', url: 'https://github.com/Naus4816/TP7'
            }
        }

        stage('Build the application') {
            steps {
                bat 'mvn clean install'
            }
        }

        
        stage('Unit Test Execution') {
            steps{
                bat 'mvn test'
            }
        }
        
        stage('Build the docker image') {
            steps {
                withCredentials([string(credentialsId:'dockerhubpass', variable:'dockerHubPass')]) {
                    bat "docker login -u naus4816 -p $dockerHubPass"
                    bat "docker build --tag naus4816/triang7:1.0.0 ."
                }
                bat 'docker push naus4816/triang7:1.0.0'
            }
        }
    }
    post{
            failure{
                emailext body: 'Ce Build $BUILD_NUMBER a échoué',
                recipientProviders:[requestor()], subject: 'build', to:
                    'naus4816@gmail.com
                }
        }
}
