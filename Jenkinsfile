def projectName = 'sea-bass'
def version = "0.0.${currentBuild.number}"
def dockerImageTag = "${projectName}:${version}"

pipeline {
  agent any

  stages {
     stage('Build docker image') {
          // this stage also builds and tests the Java project using Maven
          steps {
            sh "docker build -t ${dockerImageTag} ."
            //sh 'docker stop \$(docker ps -a -q)'
            //sh 'docker rm \$(docker ps -a -q)'
            sh "docker run -t -d --name test6 ${dockerImageTag} sleep 300"
            
            
            sh 'docker exec test6 /bin/bash -c "pytest"'
          }
      }
    
  
      
    stage('Deploy Container To Openshift') {
      steps {
        sh "oc login https://linuxops-miss35.conygre.com:8443 --username admin --password admin --insecure-skip-tls-verify=false"
        sh "oc project ${projectName} || oc new-project ${projectName}"
        sh "oc delete all --selector app=${projectName} || echo 'Unable to delete all previous openshift resources'"
        sh "oc new-app ${dockerImageTag} -l version=${version}"
        
      }
    }
  }
}
