pipeline {
   agent { any }

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
