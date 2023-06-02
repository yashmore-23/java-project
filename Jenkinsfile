pipeline {
   environment {
     git_url = "https://github.com/salilkul/java-project.git"
     git_branch = "master"
   }

  agent any
 
  stages {
    stage('Pull Source') {
      steps {
        git credentialsId: 'ff6d7224-fa35-465b-a41b-93ff8a6d2663', branch: "${git_branch}", url: "${git_url}"
       
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
                 withCredentials([usernamePassword(credentialsId: 'df745b56-66b0-4985-baf6-0d5c11c7191a', passwordVariable: 'Password', usernameVariable: 'Username')]) {
                 sh "sudo docker login -u ${env.Username} -p ${env.Password}"
                 sh "sudo docker image tag myjava-image salilkul87/myjava-image:${BUILD_NUMBER}"
                 sh "sudo docker image push salilkul87/myjava-image:${BUILD_NUMBER}" 
               } 
             }  
          }
    }

  post {
    always {
      deleteDir() /* cleanup the workspace */
    }
  }
  }
