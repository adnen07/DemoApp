pipeline {
    environment{
    registry="adnenettayeb/demoapp"
    registryCredential='dockerHub'
    dokerImage="demoapp"
}
    agent any
    stages {
        stage("Cloning Project"){
            steps {
                git branch: 'main',
                url: 'https://github.com/adnen07/DemoApp.git';
                echo 'checkout stage'
            }
        }
       
        stage ('maven clean') {
      steps {
        sh 'mvn clean -e'
        echo 'Build stage done'
      }
    }
   
        stage("compile Project"){
        steps {
            sh 'mvn compile -X -e'
            echo 'compile stage done'
            }
        }
        stage("unit tests"){
            steps {
                 sh 'mvn test'
                  echo 'unit tests stage done'
            }
        }
         
        stage('maven package') {
             steps {
               sh 'mvn package'
          }
       }
       stage("docker build") {
                       steps{
                         script {
                            dockerImage = docker.build registry + ":$BUILD_NUMBER"
                       }
                 }
       }
       stage("docker push") {
              steps{
                 script {
                 docker.withRegistry( '', registryCredential ) {
                  dockerImage.push()
    }
    }
   }
}
    stage('Cleaning up') {
             steps{
             sh "docker rmi $registry:$BUILD_NUMBER"
}
}
    }
}
