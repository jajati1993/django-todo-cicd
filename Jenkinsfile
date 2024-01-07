pipeline {
    agent any
    stages{
        stage ("clone") {
            steps {
                echo "cloning  the code"
                git url:"https://github.com/jajati1993/django-todo-cicd.git", branch: "develop"
            }
            
        }
        stage ("build") {
            steps {
                echo "building the code"
                sh "docker build . -t django-app"
            }
            
        }
        stage ("push") {
            steps {
                echo "pushing the code"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                   sh "docker tag django-app ${env.dockerHubUser}/django-app:latest"
                   sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"  
                   sh "docker push ${env.dockerHubUser}/django-app:latest"
                }
               
                
            }
            
        }
        stage ("deploy") {
            steps {
                echo "deploying the code"
                sh "docker-compose down"
                sh "docker-compose up -d "
            }
            
        }
    }
}
