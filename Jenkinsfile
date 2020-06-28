pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker IMage') {
            when {
                branch 'master'
            }
            steps {
                app = docker.build('clairebear/train-schedule')
                app.inside {
                    sh 'echo $(curl http://localhost:8080)'
                }
            }
        }
    }
}
