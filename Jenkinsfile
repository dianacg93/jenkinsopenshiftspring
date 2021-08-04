def projectName = 'demospring'
def version = "0.0.${currentBuild.number}"
def dockerImageTag = "${projectName}:${version}"

pipeline {
  agent any
  //environment{
      //DOCKER_CRED = credentials('docker-cred')
  //}

  stages {
/*    stage('Test') {
      steps {
        sh 'chmod a+x mvnw'
        sh './mvnw test'
      }
    }

    stage('Build') {
      steps {
        sh './mvnw package'
      }
    }
   */
     stage('Build docker image') {
          // this stage also builds and tests the Java project using Maven
          steps {
            sh "docker build -t ${dockerImageTag} ."
          }
      }
       /*
      stage('Push to Dockerhub'){
       steps {
             sh "docker tag ${dockerImageTag} bhashend/${dockerImageTag}"
             sh "docker login -u ${DOCKER_CRED_USR} -p ${DOCKER_CRED_PSW}"
             sh "docker push bhashend/${dockerImageTag}"
         }
      }*/

    stage('Deploy Container To Openshift') {
      steps {
        sh "oc project ${projectName} || oc new-project ${projectName}"
        //sh "oc get service mongo || oc new-app mongo"
        sh "oc delete all --selector app=${projectName} || echo 'Unable to delete all previous openshift resources'"
        sh "oc new-app ${dockerImageTag} -l version=${version}" //" -e DB_HOST=mongo"
        sh "oc expose svc/${projectName}"
      }
    }
  }

  //post {
    //always {
      //archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
      //archiveArtifacts artifacts: 'target/site/jacoco/**/*'
      //archiveArtifacts 'target/surefire-reports/**/*'
    //}
  //}
}

