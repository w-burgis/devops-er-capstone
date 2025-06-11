pipeline {
    agent any
    stages {
        stage('Clone the devops repo') {
            steps {
                echo 'Cloning devops project repository'
                git branch: 'main',
                    url: 'https://github.com/w-burgis/devops-er-capstone.git'
                echo 'Was repo cloned?'
                sh 'ls -a'
            }
        }
        stage('Build backend Docker image') {
            steps {
                script {
                    echo 'Building backend Docker image'
                    dir('backend') {
                        sh 'ls -a'
                        def imageName = 'wburgis/devops-er-backend'
                        def imageTag = '1.0'
                        sh "docker build -t ${imageName}:${imageTag} ."
                    }
                }
            }
        }
        stage('Push backend Docker image') {
            steps {
                withCredentials([usernamePassword(credentialsId: '5cd81998-a923-4dc4-8b0c-a3d5239f9661', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push wburgis/devops-er-backend:1.0
                    '''
                }
            }
        }
        stage('Build frontend Docker image') {
            steps {
                script {
                    echo 'Building frontend Docker image'
                    dir('frontend') {
                        sh 'ls -a'
                        def imageName = 'wburgis/devops-er-frontend'
                        def imageTag = '1.0'
                        sh "docker build -t ${imageName}:${imageTag} ."
                    }
                }
            }
        }
        stage('Push frontend Docker image') {
            steps {
                withCredentials([usernamePassword(credentialsId: '5cd81998-a923-4dc4-8b0c-a3d5239f9661', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push wburgis/devops-er-frontend:1.0
                    '''
                }
            }
        }
    }
}
