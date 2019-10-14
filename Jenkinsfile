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
    curl -v -u trainee:trainee --upload-file test-0.0.1.jar http://ec2-18-224-155-110.us-east-2.compute.amazonaws.com:8081/nexus/#view-repositories;devopstraining~browsestorage
          }}

}
}
