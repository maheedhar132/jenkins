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
       stage("Quality Gate"){
              timeout(time: 1,unit: 'HOURS' ){
                     def qg= waitForQualityGate()
                     if(qg.status != 'OK' )
                     {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                     }
              }
       }
   stage('Nexus Artifact Upload') {
          steps{
             withCredentials([usernamePassword(credentialsId: 'nexus-credentials', passwordVariable: 'pass', usernameVariable: 'userId')]) {
            sh   'curl -u $userId:$pass --upload-file /var/lib/jenkins/workspace/SonarGate/target/test-0.0.1.jar http://ec2-18-224-155-110.us-east-2.compute.amazonaws.com:8081/nexus/content/repositories/devopstraining/freestyle'
             
             }}
          }

}
}
