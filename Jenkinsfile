pipeline {
   environment {
     git_url = "https://github.com/yashmore-23/java-project.git"
     git_branch = "master"
   }

  agent {label 'dev'}
  //agent any
  stages {
    stage('Pull Source') {
      steps {
        git credentialsId: '04dea6e1-2c70-4574-bb87-589c84f2c7d0', branch: "${git_branch}", url: "${git_url}"
       
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
                 withCredentials([usernamePassword(credentialsId: '82377f3d-eaad-4d26-8e68-bdb06c13e4d8', passwordVariable: 'Password', usernameVariable: 'Username')]) {
                 sh "sudo docker login -u ${env.Username} -p ${env.Password}"
                 sh "sudo docker image tag myjava-image salilkul87/myjava-image:test"
                 sh "sudo docker image tag myjava-image salilkul87/myjava-image:${BUILD_NUMBER}"
                 sh "sudo docker image push salilkul87/myjava-image:${BUILD_NUMBER}" 
               } 
             }  
          }
      stage('Deploy app') {
         steps {
           sh 'ls -ltr'
           //sh 'kubectl apply -f app-deploy.yaml'
            sh 'sudo docker container run -d --name testcont salilkul87/myjava-image:test'
        }
     }
    }

//  post {
//    always {
//      deleteDir() /* cleanup the workspace */
//    }
//  }
 }
