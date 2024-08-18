pipeline {
    agent any
    stages {
        stage("clone project"){
            steps {
                echo "Project is getting cloned"
                git url:"https://github.com/CodeForFun-JayeshP/react_django_demo_app.git", branch:"main"
            }
        }
        stage("build"){
            steps {
                echo "image build for project"
                sh "docker build . -t django-demo:latest"
            }
        }
        stage("Push"){
            steps {
                echo "push image build to DockerHub"
                script{
                    withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                        sh "docker tag django-demo:latest ${env.dockerHubUser}/django-demo:latest"
                        sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                        sh "docker push ${env.dockerHubUser}/django-demo:latest"
                    }
                }
            }
        }
        stage("Deployed"){
            steps{
                sh "docker run -d -p 8001:8001 django-demo:latest"
            }
        }
    }
}
