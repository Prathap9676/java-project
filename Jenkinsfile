pipeline {
   environment {
     git_url = "https://github.com/Prathap9676/java-project.git"
     git_branch = "main"
   }

  agent {label 'NODE2'}
  //agent any
  stages {
    stage('Pull Source') {
      steps {
        git credentialsId: '94126476-c1e6-4f2d-844f-c4eb1a682519', branch: "${git_branch}", url: "${git_url}"
       
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
                 sh "sudo docker image tag myjava-image prathap9676/myjava-image:test"
                 sh "sudo docker image tag myjava-image prathap9676/myjava-image:${BUILD_NUMBER}"
                 sh "sudo docker image push prathap9676/myjava-image:${BUILD_NUMBER}" 
               } 
             }  
          }
      stage('Deploy app') {
         steps {
           sh 'ls -ltr'
           //sh 'kubectl apply -f app-deploy.yaml'
            sh 'sudo docker container run -d --name testcont-${BUILD_NUMBER} prathap9676/myjava-image:test'
        }
     }
    }

//  post {
//    always {
//      deleteDir() /* cleanup the workspace */
//    }
//  }
 }
