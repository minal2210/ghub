@Library('pipeline-library-demo')_
def my_ref_func(String name){
   echo "Hello ${name}."
}

node {
   def mvnHome
   stage('Preparation') { // for display purposesmy_ref_func "intelipath"
      my_ref_func "intelipath"
      // Get some code from a GitHub repository
      git 'https://github.com/minal2210/ghub.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.  
      echo  "${params.MavenHome}"
      mvnHome = tool 'Maven_3.3.9'
   }
   stage('Build') {
      // Run the maven build
      withEnv(["MVN_HOME=$mvnHome"]) {
         if (isUnix()) {
            sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean package'
         } else {
            bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
         }
      }
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archiveArtifacts 'target/*.jar'
   }
}
