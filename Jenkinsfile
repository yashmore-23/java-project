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
        git credentialsId: 'c5e89767-e12e-49b8-becd-90852e5d3195', branch: "${git_branch}", url: "${git_url}"
       
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
                 withCredentials([usernamePassword(credentialsId: '615498df-92e8-4905-9414-1a11f6e76dab', passwordVariable: 'Password', usernameVariable: 'Username')]) {
                 sh "sudo docker login -u ${env.Username} -p ${env.Password}"
                 sh "sudo docker image tag myjava-image ymore23/myjava-image:test"
                 sh "sudo docker image tag myjava-image ymore23/myjava-image:${BUILD_NUMBER}"
                 sh "sudo docker image push ymore23/myjava-image:${BUILD_NUMBER}" 
               } 
             }  
          }
      stage('Deploy app') {
         steps {
           sh 'ls -ltr'
           //sh 'kubectl apply -f app-deploy.yaml'
            sh 'sudo docker container run -d --name testcont ymore23/myjava-image:test'
        }
     }
    }

//  post {
//    always {
//      deleteDir() /* cleanup the workspace */
//    }
//  }
 }
