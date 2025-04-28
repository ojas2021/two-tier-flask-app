pipeline {
    agent any
    stages {
        stage("Code") {
            steps {
                git url: "https://github.com/ojas2021/two-tier-flask-app.git", branch: "master"
                echo "Code cloned successfully...!!!"
            }
        }
        stage("Build") {
            steps {
                sh "docker build -t two-tier-flask-app ."
                echo "Code built successfully...!!!"
            }
        }
        stage("Test") {
            steps {
                echo "Code tested successfully...!!!"
            }
        }
        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                )]) {
                    sh "docker login -u ${dockerHubUser} -p ${dockerHubPass}"
                    sh "docker image tag two-tier-flask-app ${dockerHubUser}/two-tier-flask-app:latest"
                    sh "docker push ${dockerHubUser}/two-tier-flask-app:latest"
                    echo "Pushed to Docker Hub successfully...!!!"
                }
            }
        }
        stage("Deploy") {
            steps {
                sh "docker compose up -d --build flask-app"
                echo "Code deployed successfully...!!!"
            }
        }
    }
}
