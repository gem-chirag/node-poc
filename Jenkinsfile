node {
    def DOCKER_IMAGE = "node-poc-image"
    def DOCKER_REGISTRY = "localhost:8082/repository/node-poc/"
    def NEXUS_CREDENTIALS = 'nexus-cred'
    def GIT_REPO = 'https://github.com/gem-chirag/node-poc.git'
    def KUBE_CONFIG_PATH = "C:\\Users\\Chirag.Thakur\\.kube\\config"

    stage('Clone repository') {
        checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: GIT_REPO]]])
    }

    stage('Build Docker image') {
        bat 'docker build -t node-poc-image:latest .'
        echo "Build Successful......"
    }

    stage('Build Docker image') {
        bat 'minikube cache add node-poc-image:latest'
        echo "Image successfully added to minikube cache...."
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
            bat "kubectl get pods"
        }
        echo "Deployment Successful....."
    }
}
