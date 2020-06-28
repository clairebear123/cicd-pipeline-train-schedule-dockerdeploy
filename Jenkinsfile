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
                script {
                    app = docker.build('clairebear/train-schedule')
                    app.inside {
                        sh 'echo $(curl http://localhost:8080)'
                    }
                }
            }
        }
        stage('Push Docker IMage') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('http://registry.hub.docker.com','docker_hub_login')
                    docker.push('${env.BUILD_NUMBER}')
                    docker.push('latest')
                }
            }
        }
    }
}
