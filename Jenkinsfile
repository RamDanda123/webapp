pipeline {
    agent none
    tools {
        maven "maven-3.9.6"
    }

    stages {
        stage('Run Tests') {
            parallel {
                stage('Build1') {
                    agent { label 'aws-labels' }
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
                stage('Build2') {
                    agent { label 'maven-labels' }
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
    }
}