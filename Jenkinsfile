pipeline {
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
        stage('mvn build') {
            steps {
            sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
            post {
                //if maven build was able to run the test we will create a test report and archive the jar in local machine
                success {
                    junit '**/target/surefire-reports/*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
         stage('checkstyle') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage('checkstyle Report') {
            steps {
                recordIssues(tools: [checkStyle(pattern: 'target/checkstyle-result.xml')])
            }
        }
         stage('code coverage') {
            steps {
                jacoco()
            }
        }
        stage('sonar scanner') {
            steps {
                sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=mymovieapp -Dsonar.host.url=http://20.84.43.19:9000 -Dsonar.login=sqp_34c66075c9665afde34209f1483df01fda0046a4'
            }
        }
        stage ('Nexus upload')  {
          steps {
          nexusArtifactUploader(
          nexusVersion: 'nexus3',
          protocol: 'http',
          nexusUrl: '52.146.37.133:8081',
          groupId: 'websocket-demo',
          version: '0.0.1-SNAPSHOT',
          repository: 'maven-snapshots',
          credentialsId: 'nexus-cred',
          artifacts: [
            [artifactId: 'websocket-demo',
             classifier: '',
             file: 'target/websocket-demo-0.0.1-SNAPSHOT.jar',
             type: 'jar']
        ]
        )
          }
     }
        
    }
}
