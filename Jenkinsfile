node {
    def DOCKER_IMAGE = "node-poc-image"
    def DOCKER_REGISTRY = "localhost:8081/repository/node-poc/"
    def NEXUS_CREDENTIALS = 'nexus-cred'
    def GIT_REPO = 'https://github.com/gem-chirag/node-poc.git'
    def KUBE_CONFIG_PATH = "C:\\Users\\Chirag\\.kube\\config"

    stage('Clone repository') {
        checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: GIT_REPO]]])
    }

    stage('Build Docker image') {
        bat 'docker build -t node-poc-image:latest .'
        echo "Build Successful......"
    }

    stage('Push Docker image to Nexus') {
        docker.withRegistry("http://${DOCKER_REGISTRY}", "${NEXUS_CREDENTIALS}") {
            docker.image("${DOCKER_IMAGE}:latest").push()
        }
        echo "Successfully pushed to Nexus repository"
    }

    stage('Deploy to Minikube') {
        withEnv(["KUBECONFIG=${KUBE_CONFIG_PATH}"]) {
            bat "kubectl version"
            bat "kubectl apply -f deployment.yaml"
        }
        echo "Deployment Successful....."
    }
}
