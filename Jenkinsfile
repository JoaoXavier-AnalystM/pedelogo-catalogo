pipeline {
    agent any

    stages {

        stage('Get Source') {
            steps {
                git url: 'https://github.com/JoaoXavier-AnalystM/pedelogo-catalogo.git', branch: 'main'
            }
        }

        stage('Docker Build Image') {
            steps {
                script {
                    dockerapp = docker.build("joaotixavier/api-produto:${env.BUILD_ID}",
                      '-f ./src/PedeLogo.Catalogo.Api/Dockerfile .')
                }
            }
        }

        stage('Docker Push Image') {
            steps {
                script {
                        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

        stage('Apply Kubernetes Files') {
            steps {
                withKubeConfig([credentialsId: 'kubeconfig']) {
                sh 'cat deployment.yaml | sed "s/{{BUILD_NUMBER}}/$BUILD_NUMBER/g" | kubectl apply -f -'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
    post {
        success {
        slackSend(message: "sbmvn Pipeline is successfully completed.")
    }
        failure {
        slackSend(message: "sbmvn Pipeline failed. Please check the logs.")
    }
}
}
