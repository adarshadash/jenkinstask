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
               echo "change"
               checkout(poll:false,
                        changelog:false,
                   scm:[$class: 'GitSCM', branches: [[name: '*/main']], clean:true,
                         doGenerateSubmoduleConfigurations: false, 
                         extensions: [ [$class: 'DisableRemotePoll'],[$class: 'PathRestriction', excludedRegions: 'application.yaml', includedRegions: 'src/.*']],
                       userRemoteConfigs: [[credentialsId: 'jenkins_id', url: 'git@github.com:adarshadash/jenkinstask.git']] 
                        ]
                     )     
            }
        }
        stage('checking function from shared Library') {
            steps{
                  welcome("Adarsha dash") 
               script{
               calculator.add(4,5)
               calculator.multiply(5,6)
               welcome.anothercall("OtherCall-Name")
               version = welcome.incrementbyone(welcome.getBuild())
               welcome.updateApplication()
               sh "git add ${env.WORKSPACE}/application.yaml"
               sh "cat ${env.WORKSPACE}/application.yaml"
              /* sh "git config --global user.email 'adi.dash880@gmail.com'"
               sh "git config --global user.name 'adarshadash'"
               sh "git remote set-url origin git@github.com:adarshadash/jenkinstask.git"    */
                   
               sh "git add ."
               sh "pwd"
               sh "git commit -m 'increament commit message ${version}'"
               sh "git tag -a -m 'version ${version}' ${version}"    
               sh "git push origin HEAD:${env.BRANCH_NAME}"
               }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {   
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
