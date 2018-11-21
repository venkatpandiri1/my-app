
node('master') {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      //git 'https://github.com/jglick/simple-maven-project-with-tests.git'
       git 'https://github.com/venkatpandiri1/my-app.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'maven'
   }
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
        // sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
          sh "'${mvnHome}/bin/mvn' -Pmyapp-deploy -Dmaven.test.failure.ignore clean install"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   stage('Uploader'){
      nexusArtifactUploader(
         artifacts: [[
                     artifactId: 'simple-maven-project-with-tests',
                     classifier: '',
                     file: '/home/ubuntu/jenkinsslave/workspace/nexus/target/my-app-1.0-SNAPSHOT.jar',
                     type: 'jar'
                   ]],
                   credentialsId: 'nexusartifact',
                   groupId: 'test',
                   nexusUrl: 'ec2-34-207-232-28.compute-1.amazonaws.com:8081/nexus',
                   nexusVersion: 'nexus2',
                   protocol: 'http',
                   repository: 'myapp-snapshots',
                   version: '1.0-SNAPSHOT'          
           ) 
      }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
}
