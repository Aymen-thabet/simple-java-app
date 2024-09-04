pipeline{
    agent{
        label 'aws-agent'
    }
    stages{
        stage('build'){
            steps{
                script{
                    sh 'docker build -t java-app .'
                }
            }
        }

        stage('push'){
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: '3dabd894-53a9-4858-97b0-2df256b3a9d4', passwordVariable: 'Password', usernameVariable: 'Username')]) {
                    sh 'docker login --username $Username --password $Password'
                    sh 'docker tag java-app $Username/java-app'
                    sh 'docker push $Username/java-app'
                    }
                }
            }
        }

        stage('deploy'){
            steps{
                script{
                    withAWS(credentials: 'EKS-user', region: 'eu-north-1') {
                    sh 'aws eks update-kubeconfig --region eu-north-1 --name Jenkins-test'
                    sh 'kubectl apply -f ./k8s/deployment.yaml --validate=false'
                    }
                }
            }
        }
    }
}