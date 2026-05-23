pipeline {

    agent any

    tools {
        maven 'Maven'
        jdk 'JDK21'
    }

    stages {

        stage('Checkout') {
            steps {
                // Best practice: automatically grabs your repository from the Jenkins job UI
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                // Copies the newly packaged file into Tomcat's deployment directory as puzzle.war
                sh 'cp target/*.war /var/lib/tomcat10/webapps/puzzle.war || true'
            }
        }

    }

    post {
        success {
            emailext (
                subject: "SUCCESS: ${JOB_NAME} #${BUILD_NUMBER}",
                body: """
Build Successful!

Photo Puzzle Game deployed successfully.

Play the Game here:
http://localhost:8091/puzzle/
""",
                to: "djhrishikesh2003@gmail.com"
            )
        }

        failure {
            emailext (
                subject: "FAILED: ${JOB_NAME} #${BUILD_NUMBER}",
                body: "Build Failed! Check the Jenkins console logs to find the issue.",
                to: "djhrishikesh2003@gmail.com"
            )
        }
    }
}
