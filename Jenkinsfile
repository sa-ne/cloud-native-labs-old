pipeline {
  agent {
      label 'maven'
  }
  stages {
    stage('Build Image') {
      steps {
        script {
          openshift.withCluster() {
            def bc = openshift.startBuild("inventory")
            bc.logs('-f')
          }
        }
      }
    }
    stage('Deploy') {
      steps {
        script {
          openshift.withCluster() {
            def dc = openshift.selector("dc", "inventory")
            dc.rollout().latest()
            dc.rollout().status()
          }
        }
      }
    }
  }
}
