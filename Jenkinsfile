pipeline {
    agent any

    environment {
        BACKEND_BRANCH = params.BACKEND_BRANCH
        FRONTEND_BRANCH = params.FRONTEND_BRANCH
        }

    tools {
        maven "maven3"
    }

    stages {
        stage('Checkout Backend Code') {
            steps {
                dir('backend') {
                    script {
                        git branch: "${BACKEND_BRANCH}", url: 'https://github.com/maazounimen/contact-backend-service.git'
                    }
                }
            }
        }

        stage('Build Backend') {
            when {
                anyOf {
                    branch 'main'
                    branch 'stage'
                }
            }
            steps {
                dir('backend') {
                    script {
                        sh "mvn -DskipTests clean install"
                    }
                }
            }
        }

        stage('Build Docker Image - Backend') {
            steps {
                script {
                    docker.build("imenmz/contact_backend:$BUILD_ID")
                    docker.push("imenmz/contact_backend:$BUILD_ID")
                }
            }
        }


/*         stage('Deploy to Kubernetes') {
            steps {
                script {
                // tester en local
                    sh 'kubectl apply -f ./Deployment.yml'
                    sh 'kubectl apply -f ./contact_ui_deployment.yml'
                }
            }
        } */
    }
}
