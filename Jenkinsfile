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
     nexusArtifactUploader artifacts: [[artifactId: 'test', classifier: '', file: '/var/lib/jenkins/workspace/SonarGate/target/test-0.0.1.jar', type: 'jar']], credentialsId: 'nexus-credentials', groupId: 'falcons.devops', nexusUrl: 'http://ec2-18-224-155-110.us-east-2.compute.amazonaws.com:8081/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'devopstraining', version: '0.0.1'
          }}

}
}
