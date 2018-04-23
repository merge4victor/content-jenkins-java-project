pipeline {
  agent none

  environment {
    MAJOR_VERSION = 1
  }

  stages {

    stage('Say Hello') {
      agent {
        label 'Linux'
      }
      steps {
        //sayHello 'Awesome Student!'
        echo "Say Hello"
      }
    }

    stage('Unit Tests') {
      agent {
        label 'Linux'
      }
      steps {
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }
    }

    stage('build') {
      agent {
        label 'Linux'
      }
      steps {
        sh 'ant -f build.xml -v'
      }
      post {
        success {
          archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
        }
      }
    }

    stage('deploy') {
      agent {
        label 'Linux'
      }
      steps {
        sh "if [ ! -d '/var/www/html/rectangles/all/${env.BRANCH_NAME}' ]; then mkdir /var/www/html/rectangles/all/${env.BRANCH_NAME}; fi"
        sh "cp dist/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/${env.BRANCH_NAME}/"
      }
    }

    stage("Running on CentOS") {
      agent {
        label 'Linux'
      }
      steps {
        sh "wget  http://10.20.10.65/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar 3 4"
      }
    }

   stage("Test on Docker") {
      agent {
        docker 'openjdk:8u162-jre'
      }
      steps {
        sh "wget http://10.20.10.65/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar 3 4"
      }
    }

    stage('Promote to Green') {
      agent {
        label 'Linux'
      }
      when {
        branch 'master'
      }
      steps {
        sh "cp /var/www/html/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar"
      }
    }

    stage('Promote Development Branch to Master') {
      agent {
        label 'windows'
      }
	when{
	branch 'development'
	}
      steps {
        echo "Stashing Any Local Changes"
        bat 'git stash'
        echo "Checking Out Development Branch"
        bat 'git checkout development'
        echo 'Checking Out Master Branch'
        bat 'git pull origin'
        bat 'git checkout master'
        echo 'Merging Development into Master Branch'
        bat 'git merge development'
        echo 'Pushing to Origin Master'
        //bat 'git push origin master'
        echo 'Tagging the Release'
        //cmd "git tag rectangle-${env.MAJOR_VERSION}.${env.BUILD_NUMBER}"
        //cmd "git push origin rectangle-${env.MAJOR_VERSION}.${env.BUILD_NUMBER}"
      }

    }

  }

}
