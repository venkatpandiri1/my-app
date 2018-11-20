
node('maven-label') {
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
          sh "'${mvnHome}/bin/mvn' -Pmyapp-deploy -Dmaven.test.failure.ignore clean deploy"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   nexusArtifactUploader {
        nexusVersion('nexus2')
        protocol('http')
        nexusUrl('ec2-54-89-82-138.compute-1.amazonaws.com:8081/nexus')
        groupId('com.mycompany.app')
        version('2.14.10-01')
        repository('myapp-releases')
        credentialsId('myappuser')
        artifact {
            artifactId('my-app')
            type('jar')
            //classifier('debug')
            file('my-app-1.0.jar')
        }
        
      }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
}
