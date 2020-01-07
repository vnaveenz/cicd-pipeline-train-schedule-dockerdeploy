pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                echo 'Running build automation'
                sh 'gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('build docker image') {
            when {
                branch 'master'
            }
            script {
                app = docker.build('vnaveenz/train-schedule')
                app.inside {
                    sh 'echo $(curl localhost:8080)'
                }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.example.com', 'docker_hub_login')
                    app.push("${env.BUILD_ID}")
                    app.push("latest")
                }
            }
        }
    }

}
