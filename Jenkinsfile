pipeline {
    environment {
        registry = "20.232.153.16:8085/chatappnew"
        registryCredential = 'nexushub'
         dockerImage = ''
    }
    agent any 
    // agent is where my pipeline will be eexecuted
    tools {
        //install the maven version configured as m2 and add it to the path
        maven "m3"
    }
   stages {
        stage('pull from scm') {
            steps {
           git credentialsId: 'git-token', url: 'https://github.com/gopal1409/springchat1.git'
            }
        }
         stage('build it') {
            steps {
            sh 'mvn clean package'
            }
        }
        stage('docker image') {
            steps {
                script {
                   dockerImage=docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
       stage('docker push') {
            steps {
                script {
                  docker.withRegistry('http://20.232.153.16:8085',registryCredential) {
                      dockerImage.push()
                  }
                }
            }
        }
   }
}
