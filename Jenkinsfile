pipeline {
  environment {
    registry = "patrickmerrick23/spring-petclinic-hub"
    registryCredential = 'docker-hub'
    dockerImage = ''
  }
  agent any
  tools {
    maven 'Maven 3.9.4'
    jdk 'jdk-20'
  } 
  stages {
/*     stage('Cloning Git') {
      steps {
        git 'https://github.com/patrick-merrick/spring-petclinic.git'
      }
    } */
    stage('Compile') {
       steps {
         shell 'mvn clean compile' //only compilation of the code
       }
    }
    stage('Test') {
      steps {
        shell '''
        mvn clean install
        ls
        pwd
        ''' 
        //if the code is compiled, we test and package it in its distributable format; run IT and store in local repository
      }
    }
    stage("Build and start test image") {
        steps {
            shell "docker-composer build"
            shell "docker-compose up -d"
            waitUntilServicesReady
        }
      }
    }
    stage('Deploy Image') {
      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        shell "docker rmi $registry:latest"
      }
    }
  }
}
