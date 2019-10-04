pipeline{
agent any 

stages{
stage('Initialize'){
steps{
sh '''
echo "PATH = ${PATH}"
echo "M2_HOME = ${M2_HOME}"
'''
}
}
       stage ('clean'){
              steps{
       sh 'mvn clean'
              }}
stage('build'){
steps{
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
     nexusPublisher nexusInstanceId: 'nexus', nexusRepositoryId: 'devopstraining', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/test-0.0.1-SNAPSHOT.jar']], mavenCoordinate: [artifactId: 'test', groupId: 'com.maven', packaging: 'jar', version: '0.0.1-SNAPSHOT']]]
          }}

}
}
