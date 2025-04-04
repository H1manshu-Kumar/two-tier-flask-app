pipeline{
    
    agent any;
    
    stages{
        stage("Code Clone"){
            steps{
                git url: "https://github.com/H1manshu-Kumar/two-tier-flask-app", branch: "master"
            }
        }
        stage("Build"){
            steps{                
                sh "docker build -t two-tier-flask-app ."
            }
        }
        
        stage("Test"){
            steps{
                echo "Test stage"
            }
        }
        
        stage("Push to Dockerhub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"docker-hub-cred",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                )]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                }
            }
        }
        
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build"
            }
        }
    }    
}
