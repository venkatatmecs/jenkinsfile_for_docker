/* before starting the build make sure that docker service shoud run, fire following command like "sudo groupadd docker" "sudo usermod -aG docker $USER" and "chmod 777 /var/run/docker.sock"*/

pipeline {
  environment {
    registry = "venkatatmecs/testjenkins"
    registryCredential = 'dockerhub'
    
  }
  agent any
  
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/venkatatmecs/terraformdockerfile.git'
      }
    }
    
    stage('Building image') {
      steps{
        script {
        
              app = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    
    stage('Push image') {  
        steps {
            script {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') 
        {                                
        app.push("${env.BUILD_NUMBER}")                      
        app.push("latest")
        } 
            }
       
        }
     } 
          
   
   stage('Remove Unused docker image') {
      steps{
         sh "docker rmi $registry:$BUILD_NUMBER"
        }
    }  
    
  }
} 
