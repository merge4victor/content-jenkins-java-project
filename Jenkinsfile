pipeline {

  agent {
        label 'Linux'
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
}