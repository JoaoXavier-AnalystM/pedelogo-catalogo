pipeline {
    agent any

    stages {

        stage('Get Source') {
            steps {
                git url: 'https://github.com/JoaoXavier-AnalystM/pedelogo-catalogo.git', branch: 'master'
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
    }
}
