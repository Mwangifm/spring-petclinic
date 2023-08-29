#!groovy

pipeline {
	  agent none
  stages {
  	stage('Maven Install') {
    	  agent {
        docker {
        	  image 'maven:3.9.3'
        }
      }
      steps {
      	sh 'mvn clean install'
      }
    }
     stage('Docker Build') {
      	agent any
      steps {
      	sh 'docker build -t mwangifm/spring-petclinic:latest .'
      }
     }
    stage('Docker Push') {
    	agent any
      steps {
      	withCredentials([usernamePassword(credentialsId: 'Dockerhub', passwordVariable: 'DockerhubPassword', usernameVariable: 'DockerhubUser')]) {
        	sh "docker login -u ${env.DockerhubUser} -p ${env.DockerhubPassword}"
          sh 'docker push mwangifm/spring-petclinic:latest'
        }
      }
    }
    post {
        // Clean after build
        always {
            cleanWs(cleanWhenNotBuilt: false,
                    deleteDirs: true,
                    disableDeferredWipeout: true,
                    notFailBuild: true,
                    patterns: [[pattern: '.gitignore', type: 'INCLUDE'],
                               [pattern: '.propsfile', type: 'EXCLUDE']])
        }
    } 
  }
} 

