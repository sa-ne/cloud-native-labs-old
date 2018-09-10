pipeline {
  agent {
      label 'maven'
  }
  stages {
    stage('Build Image') {
      steps {
        script {
          openshift.withCluster() {
            openshift.startBuild("inventory", "--follow")
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
