pipeline {

  agent {
        label 'Linux'
      }

	options{
	   buildDiscarder(logRotator(numToKeepStr: '2',artifactNumToKeepStr: '2'))
	}

	 environment {
   		 MAJOR_VERSION = 1
  		}

	stages {

    stage('Unit Tests'){
      steps{
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }
    }

		stage('build'){
			steps{
				sh '''#!/bin/bash
					echo $PATH
					ant -f build.xml -v
				'''
			}
		}

    stage('deploy'){
      steps {
        sh "cp dist/rectangle_1_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/"
        }
      }
    }
	}

	post {
		always {
			archive 'dist/*.jar'
			}
	}

}
