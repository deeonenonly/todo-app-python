
pipeline {
    agent any

    stages {
        stage('Code') {
            steps {
                git url: 'https://github.com/faizan35/todo-app-python.git',
                    branch: 'main'
                echo 'Source code checked out.'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t todo-app .'
                echo 'Docker image built successfully.'
            }
        }

        stage('Validate') {
            steps {
                sh 'docker image inspect todo-app >/dev/null'
                echo 'Docker image validation passed.'
            }
        }

        stage('Push') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerHub',
                        usernameVariable: 'DOCKERHUB_USER',
                        passwordVariable: 'DOCKERHUB_PASS'
                    )
                ]) {
                    sh '''
                        set +x
                        echo "$DOCKERHUB_PASS" |
                          docker login -u "$DOCKERHUB_USER" --password-stdin
                        docker tag todo-app "$DOCKERHUB_USER/todo-app:latest"
                        docker push "$DOCKERHUB_USER/todo-app:latest"
                        docker logout
                    '''
                }
                echo 'Image pushed to Docker Hub.'
            }
        }
    }
}
