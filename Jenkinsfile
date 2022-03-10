@Library('librarytest') _
pipeline{
    agent any
       parameters {
        string(name: "tag", defaultValue: "1.0", description: "Sample boolean parameter")
    }
      environment {
        //once you sign up for Docker hub, use that user_id here
        registry = "adarshadash/mypythonapp"
        //- update your credentials ID after creating credentials for connecting to Docker Hub
        registryCredential = 'dockerhub_secret'
        dockerImage = ''
    }
    stages {
        stage('checkout code') {
            steps{
               echo 'pulling directory form git ------>>>>>>'+ env.BRANCH_NAME
            }
        }
        stage('checking function from shared Library') {
            steps{
                  welcome("Adarsha dash") 
               script{
               calculator.add(4,5)
               calculator.multiply(5,6)
               welcome.anothercall("OtherCall-Name")
               welcome.incrementbyone(welcome.getBuild())
               welcome.updateApplication()    
               }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                  def tagtoadd = getTag()    
                  sh 'docker build -t adarshadash/sample:1.5 .'
                }
            }
        }
        stage('Deploy Docker Image') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'dockerhub_secret', variable: 'dockerhubpwd')]) {
                    sh 'docker login -u adarshadash -p ${dockerhubpwd}'
                 }  
                 sh '''docker push adarshadash/sample:1.5 '''
                }
            }
        }
    }
    post {
    always {
      cleanWs()
    }
  }
}
