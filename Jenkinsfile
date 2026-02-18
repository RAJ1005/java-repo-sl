@Library('mbp-sl@main') _

pipeline {

    agent any

    tools {
        maven 'maven399'
    }

    stages {

        stage('Build') {
            steps {
                mavenBuild()
            }
        }
    }

    post {

        success {
            script {
                emailNotification.successEmail()
            }
        }

        failure {
            script {
                emailNotification.failureEmail()
            }
        }

    }
}
