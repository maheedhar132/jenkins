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
             withCredentials([usernamePassword(credentialsId: 'trainee', passwordVariable: 'pass', usernameVariable: 'userId')]) {
        sh 'curl -u $userId:$pass sh http://ec2-18-224-155-110.us-east-2.compute.amazonaws.com:8081/nexus/#view-repositories;devopstraining~browsestorage'
                     sh 'curl -u  $userId:$pass --upload-file /var/lib/jenkins/workspace/SonarGate/target/test-0.0.1.jar http://ec2-18-224-155-110.us-east-2.compute.amazonaws.com:8081/nexus/#view-repositories;devopstraining~browsestorage'}
          }}

}
}
