pipeline {
    agent any

    environment {
       dockerImage=''
       registry='ramywageh/ramy-dockerhub'
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
        stage('Build'){
            steps{
                script{
                    dockerImage = docker.Build registry
                }
            }
        }
    }
}

      