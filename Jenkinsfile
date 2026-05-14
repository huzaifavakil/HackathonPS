pipeline {
    agent any

    environment {
        DOCKER_IMAGE     = 'maven:3.9.6-eclipse-temurin-17'
        MAVEN_OPTS       = '-Dmaven.repo.local=/root/.m2/repository'
        APP_NAME         = 'my-java-app'
        DEPLOY_ENV       = 'staging'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image "${DOCKER_IMAGE}"
                    args  '-v $HOME/.m2:/root/.m2'   // mount local .m2 cache
                }
            }
            steps {
                echo 'Running unit & integration tests...'
                sh 'mvn test ${MAVEN_OPTS}'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'   // publish test results
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}