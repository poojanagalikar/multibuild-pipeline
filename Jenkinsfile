pipeline {
    agent any
    
    tools
    {
       maven "m3"
    }
     
    stages {
      stage('checkout') {
           steps {
             
             git branch: 'main', url: 'https://github.com/poojanagalikar/chatapp-ansible-tomcat.git'
             
          }
        }
        stage('build it') {
            steps {
            sh 'mvn clean package'
            }
        }
        
   stage('ansible prov'){
            steps {
               ansiblePlaybook credentialsId: 'ansible-jenkins1', disableHostKeyChecking: true, inventory: 'client.inv', playbook: 'tomcat.yml'
            }
        }
}
}
