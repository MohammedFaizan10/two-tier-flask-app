pipeline{
    agent any;
    stages{
        stage("Code"){
           steps{
               git url: "https://github.com/MohammedFaizan10/two-tier-flask-app.git", branch: "master"
           } 
        }
        stage("Build"){
            steps{
                sh "docker build -t my-app ."
            }
        }
        stage("Test"){
            steps{
                echo "Developer test likh ke dega"
            }
        }
        stage('Push To DockerHub'){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerhubcreds",usernameVariable:"dockerhubuser",passwordVariable:"dockerhubpass")]){
                    sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass} "
                    sh "docker image tag my-app ${env.dockerhubuser}/my-app"
                    sh "docker push ${env.dockerhubuser}/my-app:latest"
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker rm -f mysql flask-app || true
                    docker volume rm demo-cicd_mysql-data || true
                    docker compose up -d --build flask-app
                    docker update --memory 400m --memory-swap 800m mysql || true

                '''
            }
        }
    }
}
