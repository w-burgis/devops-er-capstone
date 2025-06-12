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
        
        stage('Check for backend changes') {
            steps {
                script {
                    echo 'Checking for changes in backend directory'
                    def changes = sh(script: "git diff --name-only HEAD~1 HEAD | grep '^backend/' || true", returnStdout: true).trim()
                    if (changes) {
                        echo "Changes detected:\n${changes}"
                        env.BUILD_BACKEND_IMAGE = 'true'
                    } else {
                        echo "No changes in backend directory."
                        env.BUILD_BACKEND_IMAGE = 'false'
                    }
                }
            }
        }
        
        
        stage('Determine new backend image tag') {
            when {
                expression { env.BUILD_BACKEND_IMAGE == 'true' }
            }
            steps {
                script {
                    def backendImage = 'wburgis/devops-er-backend'
                    def tagsJson = sh(
                        script: "curl -s https://hub.docker.com/v2/repositories/${backendImage}/tags?page_size=100",
                        returnStdout: true
                    ).trim()
        
                    def tags = readJSON text: tagsJson
                    def versionTags = tags.results.collect { it.name }
                    
                    echo "Version tags: ${versionTags}"

                    def newTag = '1.0'
                    if (versionTags) {
                        def maxMajor = 0
                        def maxMinor = 0

                        for (tag in versionTags) {
                            def parts = tag.tokenize('.')
                            def major = parts[0].toInteger()
                            def minor = parts[1].toInteger()

                            if (major > maxMajor || (major == maxMajor && minor > maxMinor)) {
                                maxMajor = major
                                maxMinor = minor
                            }
                        }

                        newTag = "${maxMajor}.${maxMinor + 1}"
                    }

                    env.BACKEND_IMAGE_TAG = newTag
                    echo "New Docker image tag: ${env.BACKEND_IMAGE_TAG}"
                }
            }
        }

        stage('Build backend Docker image') {
            when {
                expression { env.BUILD_BACKEND_IMAGE == 'true' }
            }
            steps {
                script {
                    echo 'Building backend Docker image'
                    dir('backend') {
                        def backendImageName = 'wburgis/devops-er-backend'
                        sh "docker build -t ${backendImageName}:${env.BACKEND_IMAGE_TAG} ."
                    }
                }
            }
        }
        
        stage('Push backend Docker image') {
            when {
                expression { env.BUILD_BACKEND_IMAGE == 'true' }
            }
            steps {
                withCredentials([usernamePassword(credentialsId: '5cd81998-a923-4dc4-8b0c-a3d5239f9661', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push wburgis/devops-er-backend:${BACKEND_IMAGE_TAG}
                    '''
                }
            }
        }
        
        stage('Check for frontend changes') {
            steps {
                script {
                    echo 'Checking for changes in frontend directory'
                    def changes = sh(script: "git diff --name-only HEAD~1 HEAD | grep '^frontend/' || true", returnStdout: true).trim()
                    if (changes) {
                        echo "Changes detected:\n${changes}"
                        env.BUILD_FRONTEND_IMAGE = 'true'
                    } else {
                        echo "No changes in frontend directory."
                        env.BUILD_FRONTEND_IMAGE = 'false'
                    }
                }
            }
        }
        
        
        stage('Determine new frontend image tag') {
            when {
                expression { env.BUILD_FRONTEND_IMAGE == 'true' }
            }
            steps {
                script {
                    def frontendImage = 'wburgis/devops-er-frontend'
                    def tagsJson = sh(
                        script: "curl -s https://hub.docker.com/v2/repositories/${frontendImage}/tags?page_size=100",
                        returnStdout: true
                    ).trim()
        
                    def tags = readJSON text: tagsJson
                    def versionTags = tags.results.collect { it.name }
                    
                    echo "Version tags: ${versionTags}"

                    def newTag = '1.0'
                    if (versionTags) {
                        def maxMajor = 0
                        def maxMinor = 0

                        for (tag in versionTags) {
                            def parts = tag.tokenize('.')
                            def major = parts[0].toInteger()
                            def minor = parts[1].toInteger()

                            if (major > maxMajor || (major == maxMajor && minor > maxMinor)) {
                                maxMajor = major
                                maxMinor = minor
                            }
                        }

                        newTag = "${maxMajor}.${maxMinor + 1}"
                    }

                    env.FRONTEND_IMAGE_TAG = newTag
                    echo "New Docker image tag: ${env.FRONTEND_IMAGE_TAG}"
                }
            }
        }

        stage('Build frontend Docker image') {
            when {
                expression { env.BUILD_FRONTEND_IMAGE == 'true' }
            }
            steps {
                script {
                    echo 'Building frontend Docker image'
                    dir('frontend') {
                        def frontendImageName = 'wburgis/devops-er-frontend'
                        sh "docker build -t ${frontendImageName}:${env.FRONTEND_IMAGE_TAG} ."
                    }
                }
            }
        }
        
        stage('Push frontend Docker image') {
            when {
                expression { env.BUILD_FRONTEND_IMAGE == 'true' }
            }
            steps {
                withCredentials([usernamePassword(credentialsId: '5cd81998-a923-4dc4-8b0c-a3d5239f9661', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push wburgis/devops-er-frontend:${FRONTEND_IMAGE_TAG}
                    '''
                }
            }
        }
    }
}
