pipeline{
    
    agent {
        node{
            label "dev"
        }
    }
    
    stages{
        stage("clone code"){
            steps{
                git url: "https://github.com/JawaidAkhtar/django-notes-app.git", branch: "main"
                echo "code successfully clone"
            }
            
        }
        stage("build & test"){
            steps{
                sh "docker build -t mynotes-app-jenkins:latest ."
                echo "successfully build from docker image mynotes-app-jenkins:latest"
            }
        }
        stage("push to dockerhub"){
            steps{
                withCredentials(
                    [usernamePassword(
                        credentialsId: "dockercreds",
                        passwordVariable: "dockerHubPass",
                        usernameVariable:"dockerHubUser"
                        )
                    ]
                ){
                sh "docker image tag mynotes-app-jenkins:latest ${env.dockerHubUser}/mynotes-app-jenkins:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/mynotes-app-jenkins:latest"
                
                }
                echo "successfully pushed to dockerhub"
            }
            
        }
        
        stage("deploy"){
            steps{
                sh "docker compose up -d"
                echo "mynotes-app successfully deployed"
                
            }
        
        }
    }
}
