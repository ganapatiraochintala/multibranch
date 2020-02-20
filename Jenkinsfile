pipeline {

  agent any

  stages {
    stage('build') {
      steps {
        sh 'javac -d . src/*.java'
        sh 'echo Main-Class: Rectangulator > MANIFEST.MF'
        sh 'jar -cvmf MANIFEST.MF rectangle.jar *.class'
      }
       post {
          success {
            archiveArtifacts artifacts: 'rectangle.jar', fingerprint: true
          }
        }
    }
    stage('run') {
      steps {
        sh 'java -jar rectangle.jar 7 9'
      }
    }
    stage('Promote Development branch to Master'){

          when {

           branch 'development'

          }

        steps {
           echo "Stashing Local Changes"
           sh "git stash"
           echo "Checking out Development"
           sh 'git checkout development'
           sh 'git pull origin development'
           echo 'Checking Out Master'
           sh 'git checkout master'
           echo "Merging Development into Master"
           sh 'git merge development'
           echo "Git push to origin"
           sh "git push origin master"
         }

       }
    }
}
