@Library("Shared") _
pipeline {
    agent any;
    stages{
        stage("Code"){
            steps{
                git url:"https://github.com/digudps/two-tier-flask-app.git", branch: "master"
                }
        }
        stage("Build"){
            steps{
                
                sh "docker build --no-cache -t two-tier-flask-app ."

            }
        }
        stage("Push to docker hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                    )]){
                    
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                }

            }
        }
        stage("Test"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
        stage("Deploy"){
            steps{
                echo "DOCKER COMPOSE SE DEPLOY HO GAYA"
            }
        }
    }
}
post{
        success{
            script{
                emailext from: 'mentor@trainwithshubham.com',
                to: 'mentor@trainwithshubham.com',
                body: 'Build success for Demo CICD App',
                subject: 'Build success for Demo CICD App'
            }
        }
        failure{
            script{
                emailext from: 'mentor@trainwithshubham.com',
                to: 'mentor@trainwithshubham.com',
                body: 'Build Failed for Demo CICD App',
                subject: 'Build Failed for Demo CICD App'
            }
        }
    }
}
