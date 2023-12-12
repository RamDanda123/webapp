pipeline {
    agent { label 'aws-labels' }

    tools {
        maven "maven-3.9.6"
    }

    stages {
        stage('Build') {
            steps {
                checkout scm
                sh "mvn -Dmaven.test.failure.ignore=true -s settings.xml clean deploy"
            }
            post {
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}