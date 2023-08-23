pipeline{
    agent any
    
    stages{
        stage("code"){
            steps{
                echo "code cloning"
                git url: "https://github.com/Vishal-pixel692/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("build"){
            steps{
                echo "docker build done"
                sh "docker build . -t two-tier-app"
            }
        }
        stage("push to repository"){
            steps{
                echo "Push done"
                withCredentials([usernamePassword(credentialsId:"Dockerhub",passwordVariable:"dockerhubPass",usernameVariable:"dockerhubUser")]){
                    sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                    sh "docker tag two-tier-app ${env.dockerhubUser}/two-tier-app:latest"
                    sh "docker push ${env.dockerhubUser}/two-tier-app:latest"
                }
            }
        }
        stage("deploy"){
            steps{
                echo "deploy done on EC2"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
