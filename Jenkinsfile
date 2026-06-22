pipeline{
    agent { label "dev"};
    stages{
        stage("code"){
            steps{
                git url: "https://github.com/ramsai0206/two-tier-flask-app", branch: "master"
            }
        }
        stage("build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("push to docker"){
            steps{
                withCredentials([usernamePassword(credentialsId: "dockerHubCreds",passwordVariable: "dockerHubPass",usernameVariable: "dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${dockerHubPass}"
                    sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                }
            }
        }
        stage("test"){
            steps{
                echo "test"
            }
        }
        stage("deploy")
        {
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
   
}
