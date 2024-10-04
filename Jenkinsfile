@Library("Shared") _
pipeline{
    agent {label "purushotam"}
    
    stages{
        stage("Hello"){
            steps{
                script{
                    hello()
                }
            }
        }
        stage("Code"){
            steps{
                script{
                clone("https://github.com/PurushotamSharma/django-notes-app.git","main")
                }
            }
        }
        stage("Build"){
            steps{
                echo "This is building the code"
                sh "whoami"
                sh "docker build -t notes-app:latest ."
                
            }
        }
        stage("Push to the Docker Hub"){
            steps{
                echo "This is Pushing the image to docker hub"
                withCredentials([usernamePassword('credentialsId':"dockerhubcred",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker tag notes-app:latest ${env.dockerHubUser}/notes-app:latest"
                sh "docker push ${env.dockerHubUser}/notes-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "This is deploying the code"
                sh "docker compose down && docker compose up -d "
            }
        }
    }
}
