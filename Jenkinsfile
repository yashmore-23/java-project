pipeline {
   environment {
     git_url = "https://github.com/salilkul/java-project.git"
     git_branch = "main"
   }

 //agent {label 'dev'}
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
     
     stage('Execute'){
      steps{
        sh  "java -jar *.jar"
           }
     }


   }
  }
