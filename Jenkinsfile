pipeline {
    agent  any
    stages {
        stage("code"){
            steps {
                 echo "cloning the code"
                 git url : "https://github.com/KunjaBihariJena/django-notes-app.git", branch: "main"
            }
        }
        stage("build"){
            steps {
                echo "build the image"
                sh "docker build -t my-note-app ."
            }
        }
        stage("push to docker hub"){
            steps {
                echo "pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]) {
                sh "docker tag my-note-app ${env.dockerHubUser}/my-note-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-note-app:latest"
                }
            }
        }
        stage("deploy"){
           steps{
             echo "deploy the container"
             sh "docker-compose down && docker-compose up -d"
            
           }
        }
    }
}
