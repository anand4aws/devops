pipeline {
    agent any
    stages{
        stage("Clone the Code"){
            steps{
                echo "Cloning the code"
                git url:"https://github.com/anand4aws/devops.git", branch: "main"
            }
        }
        stage("Build"){
            steps{
                echo "Building the image"
                sh "docker build -t note-apps ."
            }
        }
        stage("Push"){
            steps{
                echo "Pushing the code to Docker Hub"
                withCredentials([usernamePassword(credentialsId:"DockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker image tag note-apps ${env.dockerHubUser}/note-apps:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/note-apps:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying the code"
                sh "docker container rm -f notes-app"
                sh "docker container run -d --name=notes-app -p 8000:8000 anandkpdocker/note-apps:latest"
            }
        }
    }
}
