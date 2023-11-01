pipeline {
  agent {
    kubernetes {
      inheritFrom 'maven'
      containerTemplate {
        name 'maven'
        image 'maven:3.8.4-jdk-11'
      }

    }

  }
  stages {
    stage('Clone repository') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: 'master']], 
                    extensions: [[$class: 'CloneOption', depth: 1, shallow: true]], userRemoteConfigs: [[url: 'https://github.com/kubesphere-sigs/demo-java']]
                ])
      }
    }

    stage('Run compile') {
      steps {
        container('maven') {
          sh 'mvn compile'
        }

      }
    }

    stage('Run test') {
      steps {
        container('maven') {
          sh 'mvn clean test'
        }

      }
    }

    stage('Run build') {
      steps {
        container('maven') {
          sh 'mvn package'
        }

      }
    }

    stage('Archive artifacts') {
      steps {
        archiveArtifacts(artifacts: 'target/*.jar', followSymlinks: false)
      }
    }

  }
}
