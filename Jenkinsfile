pipeline{
agent any 

stages{

stage('clean and build'){
steps{
       sh 'mvn clean'
       sh 'mvn -Dmaven.test.failure.ignore=true install'
}

}

stage("SonarQube analysis") {
       
            steps {
              withSonarQubeEnv('sonarqube') {
                sh 'mvn sonar:sonar'
              }
            }
          }
   stage('Nexus Artifact Upload') {
          steps{
             withCredentials([usernamePassword(credentialsId: 'nexus-credentials', passwordVariable: 'pass', usernameVariable: 'userId')]) {
            sh   'curl -u $userId:$pass --upload-file /var/lib/jenkins/workspace/SonarGate/target/test-0.0.1.jar http://3.14.251.87:8081/nexus/content/repositories/devopstraining/freestyle'
             
             }}
          }

}
}
