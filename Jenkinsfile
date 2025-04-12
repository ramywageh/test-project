pipeline {
    agent any

    environment {
        TERRAFORM_DIR = "terraform/"
        ANSIBLE_PLAYBOOK = "ansible/playbook.yml"
    }
    stages {
        stage("Prep") {
            steps {
                git(
                    url: "https://github.com/ramywageh/test-project.git",
                    branch: "main",
                    credentialsId: "GitHub",
                    changelog: true,
                    poll: true
                )
            }
        }

        /*stage("Build") {
            steps {
                withCredentials([usernamePassword(credentialsId:"docker",usernameVariable:"USER",passwordVariable:"PASS")]){
                sh 'docker build . -t ${USER}/todo-app:v1.${BUILD_NUMBER}'
                sh 'docker login -u ${USER} -p ${PASS}'
                sh 'docker push ${USER}/todo-app:v1.${BUILD_NUMBER}'
                }
            }
        }
        stage("Test") {
            steps {
                withCredentials([usernamePassword(credentialsId:"docker",usernameVariable:"USER",passwordVariable:"PASS")]){
                sh 'docker run --rm ${USER}/todo-app:v1.${BUILD_NUMBER} pytest /app'
                }
            }
        }
    }

   post {
        success {
            withCredentials([usernamePassword(credentialsId:"docker",usernameVariable:"USER",passwordVariable:"PASS")]){
                slackSend(
                    channel: "depi-project",
                    color: "good",
                    message: "${env.JOB_NAME} is succeeded. Build no. ${env.BUILD_NUMBER} " + 
                     "(<https://hub.docker.com/repository/docker/${USER}/todo-app/general|Open the image link>)"
                )
            }
        }
        failure {
            slackSend(
                channel: "depi-project",
                color: "danger",
                message: "${env.JOB_NAME} is failed. Build no. ${env.BUILD_NUMBER} URL: ${env.BUILD_URL}"
            )
        }
    }*/
}