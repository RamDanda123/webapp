pipeline {
    agent { label 'aws-labels' }
    tools {
        maven "maven-3.9.6"
    }

    stages {
        stage('Run Tests') {
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
        stage('SonarQube Analysis') {
            withSonarQubeEnv(credentialsId: 'SONAR_TOKEN') {
                sh "mvn clean verify sonar:sonar -Dsonar.projectKey=myapp"
            }
        }
    }
}