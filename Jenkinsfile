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
                sh 'mvn clean package sonar:sonar'
              }
            }
          }
   stage('Publish') {
          steps{
     nexusArtifactUploader artifacts: [[artifactId: 'test', classifier: 'jar', file: 'pom.xml', type: 'jar']], credentialsId: 'trainee', groupId: 'falcons.devops', nexusUrl: 'ec2-18-224-155-110.us-east-2.compute.amazonaws.com:8081/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'devopstraining', version: '0.0.1'
          }}

}
}
