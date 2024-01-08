pipeline {
    agent any 
    
    stages{
        stage("code"){
            steps {
                echo "Cloning the code"
                git url:"git@github.com:shivani-1314/django-notes-app.git", branch: "main"
            }
        }
        stage("build"){
            steps {
                echo "Building the code"
                sh "docker build -t my-note-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId: "dockerhub", passwordVariable: "dockerhubPass", usernameVariable: "dockerhubUser")]) {
                echo "Docker login credentials: ${env.dockerhubUser}:${env.dockerhubPass}"
                sh "docker tag my-note-app ${env.dockerhubUser}/my-note-app:latest"
                sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                sh "docker push ${env.dockerhubUser}/my-note-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
    
    
}
