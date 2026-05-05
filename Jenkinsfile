pipeline {
    agent any
    parameters {
        string(name: 'DOCKER_IMAGE_VERSION', defaultValue: '', description: 'Docker Image Version')
    }
    stages {
        stage('Update deploy.yaml') {
            steps {
                dir('backend') {
                    sh 'git checkout main'
                    sh "sed -i 's|huisuz/beatbuddy-backend:.*|huisuz/beatbuddy-backend:${params.DOCKER_IMAGE_VERSION}|g' deploy.yaml"
                    sh 'cat deploy.yaml'
                }
            }
        }
        stage('Commit & Push') {
            steps {
                sh 'git config user.name "jenkins"'
                sh 'git config user.email "jenkins@beatbuddy.com"'
                sh 'git add .'
                sh "git commit -m 'Update backend image to ${params.DOCKER_IMAGE_VERSION}'"
                sshagent(['github-beatbuddy-k8s']) {
                    sh 'git push'
                }
            }
        }
    }
}