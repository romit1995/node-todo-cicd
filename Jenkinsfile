pipeline {
    agent { label "dev-server"}
    
    stages {
        
        stage("Code"){
            steps{
                git url: "https://github.com/romit1995/node-todo-cicd.git", branch: "master"
                echo 'Code cloning is complete'
            }
        }
        stage("Build and Test"){
            steps{
                sh "docker build -t node-todo-test ."
                echo 'code build successfully'
            }
        }
        stage("Push"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker tag node-todo-test:latest ${env.dockerHubUser}/node-todo-test:latest"
                sh "docker push ${env.dockerHubUser}/node-app-test-new:latest"
                echo 'Image is pushed'
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d --no-deps --build web"
                echo 'Deployment success'
            }
        }
    }
}
