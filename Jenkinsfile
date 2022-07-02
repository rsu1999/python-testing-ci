def projectName = 'sea-bass'
def version = "0.0.${currentBuild.number}"
def dockerImageTag = "${projectName}:${version}"


pipeline {
   agent  { dockerfile true }
   

      
   stages {
      
    
      stage('Tests') {
         steps {
            sh '/bin/bash -c "pytest"'
         }
      }
   }

   post {
       always {
           junit 'latest_test_results.xml'
       }
   }
   
}
