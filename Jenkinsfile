pipeline {
    environment {
    registry = "papis84/my-repos"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
    agent any
    parameters {
        choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description: '')
        booleanParam(name: 'executeTests', defaultValue: false, description: '')
    }


    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                // Install Node.js dependencies
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            when {
                expression {
                    params.executeTests 
                }
             
            }
            steps {
                // Run tests using Jest or any other testing framework
                sh 'npm test'
            }
        }
        stage('Build and Package') {
            steps {
                // Build and package the Node.js application
                echo 'npm run build'
                
            }
        }
        stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
       stage('Deploy Image') {
      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }



