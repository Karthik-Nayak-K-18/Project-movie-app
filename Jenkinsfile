pipeline {
    agent any

    tools{
        jdk 'java-11'
        maven 'maven'
    }

    stages {
        stage ("git clone"){
            steps {
                git branch: 'main', credentialsId:'1682e59f-d8cd-4b0c-ab44-52a17e6cbb49', url: 'https://github.com/Karthik-Nayak-K-18/Project-movie-app.git'
            }
        }

        stage ("compile"){
            steps {
                sh 'mvn clean compile'
            }
        }

        stage ('test') {
            stages {
                sh 'mvn clean test'
            }
        }

        stage ("build"){
            steps{
                sh 'mvn clean package'
            }
        }

        stage ("Docker build & tag dockerfile"){
            steps{
                sh "docker build -t manojkrishnappa/movie:2 ."
            }
        }

        stage ("trivy scan"){
            steps{
                sh "trivy image --format table -o trivy-image-report.html manojkrishnappa/movie:2"
            }
        }

        stage ("containerization"){
            steps{
                sh "docker run -it -d --name ct1 -p 9002:8080 manojkrishnappa/movie:2"
            }
        }

                stage ("docker login"){
            steps{

                script {
                            withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                                sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                            }
                }
            }
        }

        stage ("docker push") {
            steps {
                sh "docker tag manojkrishnappa/movie:2 karthiknayak18/docker-website:v.1"
                sh "docker push karthiknayak18/docker-website:v.1"
            }
        }
    }
}
