pipeline {
  agent any

  environment {
       imagename = "namadidockerhub/mytomcat_application_image" //name you want to tag your image
       registryCredential = 'dockerHub' //dockerhub credential saved in jenkins to allow push
       dockerImage = ''
           }

  stages {
//build application
    stage ('Build') {
      steps {
        sh 'mvn clean package'
        echo 'Maven Build'
      }
    }

    stage('Build Test') {
            steps {
                sh 'mvn test'
                echo 'Test Analysis'
            }
        }
//build image    
    stage('Build Docker image') {
          steps{
                script {
                     dockerImage = docker.build imagename
                          }
                      }
                }
//push image to dockerhub
     stage('Push Docker Image to DockerHub') {
           steps{
               script {
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push("$BUILD_NUMBER") //push with tag of the build number (would keep all versions by tracking the build number in the repo)
                    dockerImage.push('latest') //push with tag of latest (will be updated to the latest build)
                                              }
                                    }
                             }
                  }
// remove images from the server
     stage('Remove Unused docker image') {
          steps{
              sh "docker rmi $imagename:$BUILD_NUMBER"
              sh "docker rmi $imagename:latest"
                        }
            }  
   }
}
