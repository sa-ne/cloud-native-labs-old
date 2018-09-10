pipeline {
  agent {
      label 'maven'
  }
  stages {
    stage('Build JAR') {
      steps {
        sh "cd inventory-wildfly-swarm && mvn package"
        stash name:"jar", includes:"inventory-wildfly-swarm/target/inventory-1.0-SNAPSHOT-swarm.jar"
      }
    }
    stage('Build Image') {
      steps {
        unstash name:"jar"
        script {
          openshift.withCluster() {
            openshift.startBuild("inventory", "--wait")
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
