@Library("shared") _
pipeline{
    
    agent any
    triggers {
        githubPush()   // replaces pollSCM
    }
    stages{
        stage("Hello"){
            steps{
                script{
                    hello() //Updated
                }
            }
        }
        stage("Code"){
            steps{
                script{
                    clone("https://github.com/Aryangupta0077/fullstack-docker-project.git","main")
                }
            }
        }
        stage("Build"){
            steps{
                echo "Building the code"
                bat "docker compose build"
                echo "Build complete"
            }
        }
        stage("Push the image to dockerhub"){
            steps{
                withCredentials(
                    [usernamePassword(
                        'credentialsId':"dockerHubCred", 
                        passwordVariable:"dockerHubPass", 
                        usernameVariable:"dockerHubUser")
                        ]){
                echo "Testing the code"
                bat "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                bat "docker tag frontend:latest ${env.dockerHubUser}/frontend-full-stack:latest"
                bat "docker tag backend:latest ${env.dockerHubUser}/backend-full-stack:latest"
                bat "docker push ${env.dockerHubUser}/frontend-full-stack:latest"
                bat "docker push ${env.dockerHubUser}/backend-full-stack:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploy the code"
                bat "docker-compose up -d"
                echo "Your app is running"
            }
        }
    }
}