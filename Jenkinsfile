pipeline {
   environment {
   registry = "santoshbharath/onlinebookstore"
   registryCredential = 'dockerhub'
   dockerImage = ''
   BRANCH = 'master' 
   }

    agent any                    

    stages {    
        stage('Cloning our Git') {
               steps {
                   git credentialsId: 'github', url: 'git@github.com:USBharath/mavenproject.git', branch: 'master'
                              }
    }
        stage('Maven Build') {
               steps{
                 sh "mvn clean install package"
               }
        }
    

        stage('Pulling Docker Image') {
               steps {
                   script {
                       dockerImage = docker.build registry + ":MAVEN-${BRANCH}-$BUILD_NUMBER"
                   }
               }
        }

        stage('Deploying Docker Image to Dockerhub') {
               steps {
                   script {
                       docker.withRegistry('', registryCredential) {
                       dockerImage.push()
                       }
                   }
               }
        }

        stage('Cleaning Up') {
               steps{
                 sh "docker rmi --force $registry:MAVEN-${BRANCH}-$BUILD_NUMBER"
               }
        }

        stage('Docker Image to Dockerhub') {
               steps {
                   script {
                       docker.withRegistry('', registryCredential) {
                       dockerImage.pull()
                       }
                       sh '''
                       #!/bin/bash
                        name=$(docker ps --format "{{.Names}}")
                        if [[ "$name" == *"MAVEN"* ]]
                        then
                         docker container stop $(docker container ls -q --filter name=NODEJS*)
                         docker rm --force MAVEN-${BRANCH}-$BUILD_NUMBER
                         docker run -d --name MAVEN-${BRANCH}-$BUILD_NUMBER -p 80:80  $registry:MAVEN-${BRANCH}-$BUILD_NUMBER
                        else
                         docker rm --force MAVEN-${BRANCH}-$BUILD_NUMBER
                         docker run -d --name MAVEN-${BRANCH}-$BUILD_NUMBER -p 80:80  $registry:MAVEN-${BRANCH}-$BUILD_NUMBER
                 

                        fi
                       
                        '''
                   }
               }
        }
    }
}
