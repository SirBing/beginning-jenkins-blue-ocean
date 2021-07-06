pipeline {
  agent none
  stages {
    stage('Build & Test') {
      agent {
        docker {
          args 'sh \'mvn -Dmaven.test.failure.ignore clean package\' stash(name:'
          image 'nikhilpathania/jenkins_ssh_agent'
        }

      }
      steps {
        sh 'mvn -Dmaven.test.failure.ignore clean package'
        stash(name: 'build-test-artifacts', includes: '**/target/surefire-reports/TEST-*.xml,target/*.jar')
      }
    }

    stage('Report & Publish') {
      agent {
        docker {
          image 'nikhilpathania/jenkins_ssh_agent'
        }

      }
      steps {
        unstash 'build-test-artifacts'
        junit '**/target/surefire-reports/TEST-*.xml'
        archiveArtifacts(artifacts: 'target/*.jar', onlyIfSuccessful: true)
      }
    }

  }
}