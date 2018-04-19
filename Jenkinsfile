pipeline {

  agent {
        label 'Linux'
      }

	stages {
		stage('build'){		
		
			steps{
		
				sh '''#!/bin/bash
					ant -f build.xml -v
				'''		

			}
		}
	}
}