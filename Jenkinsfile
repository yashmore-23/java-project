pipeline {
   environment {
     git_url = "https://github.com/salilkul/java-project.git"
     git_branch = "master"
   }

  //agent {label 'dev'}
  agent any
  stages {
    stage('Pull Source') {
      steps {
        git credentialsId: '86e32eb1-c84e-4535-8ac7-93d876edcbe4', branch: "${git_branch}", url: "${git_url}"
       
      }
     }
    
    stage('Maven Build') {
     steps { 
          sh "mvn clean package && cp target/*.jar . "
     }
    }
     
     stage('Docker Image Build') {     
        steps {
              sh 'sudo docker build -t myjava-image . '
               }
             }
        stage('Docker image push') {
           steps {
                 withCredentials([usernamePassword(credentialsId: 'feb2d406-8b89-41f6-9c5b-fd165a6730fd', passwordVariable: 'Password', usernameVariable: 'Username')]) {
                 sh "sudo docker login -u ${env.Username} -p ${env.Password}"
                 sh "sudo docker image tag myjava-image salilkul87/myjava-image:test"
                 sh "sudo docker image push salilkul87/myjava-image:test" 
               } 
             }  
          }
      stage('Deploy app') {
         steps {
           sh 'ls -ltr'
           sh 'kubectl apply -f app-deploy.yaml'
        }
     }
    }

  post {
    always {
      deleteDir() /* cleanup the workspace */
    }
  }
 }
