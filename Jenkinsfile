pipeline {
    agent any

    stages {
        stage("Prep") {
            steps {
                git(
                    url: "https://github.com/ramywageh/test-project.git",
                    branch: "main",
                    credentialsId: "github",
                    changelog: true,
                    poll: true
                )
            }
        }
    }
}            