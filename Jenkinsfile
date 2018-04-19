pipeline {

  agent {
        label 'Linux'
      }

	options{
	buildDiscarder(logRotator(numToKeepStr: '2',artifactNumToKeepStr: '1'))
	}

	 environment {
   		 MAJOR_VERSION = 1
  		}

	stages {
		stage('build'){		
		
			steps{
		
				sh '''#!/bin/bash
					echo $PATH
					ant -f build.xml -v
				'''		

			}
		}
	}
	post {
		always {
			archive 'dist/*.jar'
			}
	}
}