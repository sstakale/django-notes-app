pipeline {
    agent any
    
    stages{
        stage("Clone"){
            steps{
                echo "Cloning github code"
                git url: "https://github.com/sstakale/django-notes-app.git", branch:"main"
            }
        }
        stage("Buid"){
            steps{
                echo "Build the code"
                sh "docker build -t notes-app ."
            }
        }
        stage("Push"){
            steps{
                echo "Pushing image to docker hub"
                withCredentials(
                    [usernamePassword(
                        credentialsId:"DockerHub", 
                        passwordVariable:"dhubpwd", 
                        usernameVariable:"dhubuname"
                        )
                    ]) {
                        sh "docker tag notes-app ${env.dhubuname}/notes-app:latest"
                        sh "docker login -u ${env.dhubuname} -p ${env.dhubpwd}"
                        sh "docker push ${env.dhubuname}/notes-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
