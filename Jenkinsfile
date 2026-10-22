
pipeline {
    agent any

    stages {
        stage('Code') {
            steps {
                echo 'Source code checked out from the configured SCM.'
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
