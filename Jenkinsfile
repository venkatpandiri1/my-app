node('slave1') {
   def mvnHome
   def branch = master
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git branch: '${branch}', url: 'https://github.com/jglick/simple-maven-project-with-tests.git'
      //git branch: '${branch}', url: 'https://github.com/venkatpandiri1/my-app.git' 
      println test
     // git 'https://github.com/venkatpandiri1/my-app.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'maven'
   }
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
        sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package" 
         //sonar:sonar -Dsonar.host.url=http://http://ec2-54-161-114-188.compute-1.amazonaws.com:9000"
         sh "'${mvnHome}/bin/mvn' -Pmyapp-deploy -Dmaven.test.failure.ignore clean install sonar:sonar"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   stage('Upload'){
      dir("${workspace}")
      {
      nexusArtifactUploader(
                   artifacts: [[
                     artifactId: 'simple-maven-project-with-tests',
                     classifier: '',
                      file: "target/simple-maven-project-with-tests-1.0-SNAPSHOT.jar",
                     type: 'jar'
                   ]],
                   credentialsId: 'myappuser',
                   groupId: 'test',
                   nexusUrl: 'ip-172-31-37-155.ec2.internal:8082/nexus',
                   nexusVersion: 'nexus2',
                   protocol: 'http',
                   repository: 'myapp-snapshots',
                   version: '1.0-SNAPSHOT'
) 
      }
      
      }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
}
