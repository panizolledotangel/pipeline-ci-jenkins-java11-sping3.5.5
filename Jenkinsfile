pipeline {
   
   agent any

   tools {
      jdk 'jdk_11' // Use the name you configured for JDK 11 in the Jenkins tools configuration
   }

   stages {
        
      stage('Checkout') {
         steps {
               // Checkout code from version control including submodules using the scm variable
               checkout([
                  $class: 'GitSCM', 
                  branches: [[name: "${GIT_BRANCH}"]],
                  extensions: [[$class: 'SubmoduleOption', recursiveSubmodules: true]], 
                  userRemoteConfigs: scm.userRemoteConfigs
               ])
         }
      }

      stage('Build') {
            steps {
                sh 'java -version' // Verify Java version
                sh 'mvn -version'  // Verify Maven version being used
                sh 'mvn clean package' // Run your Maven build
            }
      }

      stage('Test') {
         steps {
               script {
                  sh 'mvn test'
               }
         }
      }

      post {
         always {
            // Archive the build artifacts
            archiveArtifacts artifacts: 'target/*.jar, target/*.war', allowEmptyArchive: true

            // Clean up build directory after the build
            deleteDir()
         }
      }
   }
}