pipeline {
    
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
      steps{withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
        sh 'docker build -t papis84/my-repos:njs-2.0 .'
        sh "echo $PASS | docker login -u $USER --password-stdin"
        sh 'docker push papis84/my-repos:njs-2.0'

      }
      }
    }
       stage('Deploy Image') {
      steps{
        echo 'deploying the application...'
        echo ' deploying version ${params.VERSION}'
      }
    }
    }
}
