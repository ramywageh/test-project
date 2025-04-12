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
    }
}

      