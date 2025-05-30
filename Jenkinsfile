@Library('jenkins-shared-libraries') _

pipeline{
    
    agent {label "dev"};
    
    stages{
        stage("Code Clone"){
            steps{
                script{
                    gitClone("https://github.com/H1manshu-Kumar/two-tier-flask-app", "master")
                }                
            }
        }

        stage("File System Scan using Trivy"){
            steps{
                sh "trivy fs . -o file_system_scan_results.json"
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
    
post{
    success{
        emailext body: 'Good News!! Your build is Successful!',
            subject: "Build Suscessful",
             to: '$DEFAULT_RECIPIENTS'
    }
    failure{
        emailext body: "Bad News!! Build Failed<br><b>Example</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}",
            subject: "Build Failed",
             to: '$DEFAULT_RECIPIENTS'
    }
}
}
