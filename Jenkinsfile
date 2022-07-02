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
      
      stage('Deploy Container To Openshift') {
      steps {
        sh "oc login https://linuxops-miss31.conygre.com:8443 --username admin --password admin --insecure-skip-tls-verify=true"
        sh "oc project ${projectName} || oc new-project ${projectName}"
        sh "oc delete all --selector app=${projectName} || echo 'Unable to delete all previous openshift resources'"
        sh "oc new-app ${dockerImageTag} -l version=${version}"
        sh "oc expose svc/${projectName}"
      }
    }
      
   }

   post {
       always {
           junit 'latest_test_results.xml'
       }
   }
   
}
