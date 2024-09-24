pipeline{
    agent any
    stages{
        stage("checkout"){
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github',url: 'https://github.com/VedThakkar/DockerPipeline']])
            }
        }
         stage("Build Image") {
            steps {
                script {
                    if (isUnix()) {
                        sh 'docker build -t vedthakkar/dockerpipeline .'
                    } else {
                        bat 'docker build -t vedthakkar/dockerpipeline .'
                    }
                }
            }
        }
         stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                    script {
                        if (isUnix()) {
                            sh 'docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD'
                            sh 'docker push vedthakkar/dockerpipeline'
                            sh 'docker logout'
                        } else {
                            bat 'docker login -u %DOCKERHUB_USERNAME% -p %DOCKERHUB_PASSWORD%'
                            bat 'docker push vedthakkar/dockerpipeline'
                            bat 'docker logout'
                        }
                    }
                }
            }
        }
    }
}