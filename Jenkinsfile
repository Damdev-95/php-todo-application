pipeline {
    agent any

    stages {

        stage('Initial Cleanup') {
            steps {
            dir("${WORKSPACE}") {
              deleteDir()
                }
            }
        }

        stage ('Checkout Repo'){
            steps {

                git branch: 'main', url: 'https://github.com/Damdev-95/php-todo-application.git'
            }
        }

        stage ('Build Docker Image') {
            steps {
                script {

                       sh "docker build -t damdav95/php-todo-app:${env.BRANCH_NAME}-${env.BUILD_NUMBER} ."
                }
            }
        }

        stage ('Run Container') {
            steps {
                script {

                       sh "docker run --network tooling_app_network -p 8050:8000 -d damdav95/php-todo-app:${env.BRANCH_NAME}-${env.BUILD_NUMBER} ."
                }
            }
        }

        stage ('Test-Stage-Curl') {
            steps {
                script {

                    sh "curl --version"
                    sh  "curl -I http://18.212.3.199/:8050"
                }
            }
        }


        stage ('Push Docker Image') {
            steps{
                script {
            sh "docker login -u ${env.username} -p ${env.password}"

            sh "docker push damdav95/php-todo-app:${env.BRANCH_NAME}-${env.BUILD_NUMBER}"
                }
            }
        }


        stage ('Clean Up') {
            steps{
                script {

            sh "docker system prune -af"

                }
            }
        }


        stage ('logout Docker') {
            steps {
                script {

                    sh "docker logout"

                }
            }
        }

    }
}