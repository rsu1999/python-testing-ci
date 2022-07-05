def projectName = 'sea-bass'
def version = "0.0.${currentBuild.number}"
def dockerImageTag = "${projectName}:${version}"
def max = 50
def random_num = " ${Math.abs(new Random().nextInt(max+1))}"

pipeline {
  agent any

  stages {
     stage('Build docker image') {
          // this stage also builds and tests the Java project using Maven
          steps {

            sh "docker build -t ${dockerImageTag} ."
            sh "docker run -t -d --rm --name ${random_num} ${dockerImageTag} sleep 300"
            sh 'docker exec ${random_num} /bin/bash -c "pytest"'
          }
      }
    
  
      
    stage('Deploy Container To Openshift') {
      steps {
        sh "oc login https://linuxops-miss36.conygre.com:8443 --username admin --password admin --insecure-skip-tls-verify=true"
        sh "oc project ${projectName} || oc new-project ${projectName}"
        sh "oc delete all --selector app=${projectName} || echo 'Unable to delete all previous openshift resources'"
        sh "oc new-app ${dockerImageTag} -l version=${version}"
        
      }
    }
  }
}
