pipeline {

  agent {
        label 'Linux'
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