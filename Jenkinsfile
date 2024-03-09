pipeline{
    agent any
    environment {
        PATH = "$PATH:/opt/apache-maven-3.9.6/bin"
    }
    stages{
       stage('Checkout'){
            steps{
                git 'git@github.com:bhadri12322/Sonar-Project.git'
            }
         }        
       stage('Package'){
            steps{
                sh 'mvn clean package'
            }
         }
		post {
               always {
                 junit '*/target/surefire-reports/TEST-*.xml'
         }
         }
         } 
		 
        stage('SonarQube analysis') {
//    def scannerHome = tool 'SonarScanner 4.0';
        steps{
        withSonarQubeEnv('SonarQube') { 
        // If you have configured more than one global server connection, you can specify its name
//      sh "${scannerHome}/bin/sonar-scanner"
        sh "mvn clean verify sonar:sonar -Dsonar.projectKey=sonar-project -Dsonar.host.url=http://43.204.233.102:9000 -Dsonar.login=sqp_f8168731c1ed3cf2d64a77bc3a722060f27cc03e"
    }
        }
        }
       
    stage ('Deploy to Nexus') {
            steps {
      nexusArtifactUploader(
      nexusVersion: 'nexus3',
      protocol: 'http',
      nexusUrl: '13.200.252.85:8081',
      groupId: 'myGroupId',
      version: '1.0-SNAPSHOT',
      repository: 'maven-snapshots',
      credentialsId: 'nexuscredentials',
      artifacts: [
      [artifactId: 'maven-project',
      classifier: '',
      file: 'webapp/target/webapp.war',
      type: 'war']
      ])
      }
        }
        stage ('Deploy to Prod'){
     steps {
        sh 'scp -o StrictHostKeyChecking=no /root/Sonar-Project/webapp/target/webapp.war root@15.206.117.185:/opt/apache-tomcat-8.0.52/webapps'
           }
   }
}    
}
