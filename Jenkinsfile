pipeline {
    agent any
    
    stages {
        stage("code") {
            steps {
                echo "cloning the code"
                git url:"https://github.com/aadil611/django-notes-app.git", branch: "main"
            }
        }
        stage("build") {
            steps {
                echo "building the image"
                sh ""
                sh "docker build -t my-note-app ."
            }
        }
        stage("push image to dockerhub") {
            steps {
                echo "pushing to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dhPass",usernameVariable:"dhUser")]) {
                    sh "docker tag my-note-app ${env.dhUser}/my-note-app:latest"
                    sh "docker login -u ${env.dhUser} -p ${dhPass}"
                    sh "docker push ${env.dhUser}/my-note-app:latest"
                }
                
            }
        }
        stage("deploy") {
            steps {
                echo "deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
    
}
