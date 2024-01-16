pipeline {
    environment {
        doError = '0'
        BRANCH_NAME = "${GIT_BRANCH.split("/")[1]}"
    }
    
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main'], [name: '*/dev']], extensions: [], userRemoteConfigs: [[credentialsId: 'Github-Token', url: 'https://github.com/Anmewzaa/FutureSkill-DevOps-Course.git']])
            }
        }
        stage('Build and Push Docker Image') {
            when { expression { env.BRANCH_NAME != 'main'} }
            steps {
                script {
                    env.VERSION = "v0.0.${BUILD_NUMBER}"
                    sh('''
                        docker build -t testapi:${VERSION} -f TestingAPI/Dockerfile .
                        docker tag testapi:${VERSION} punyakon/testapi:${VERSION}
                        docker push punyakon/testapi:${VERSION}
                    ''')
                }
            }
        }
        stage('Deploy To Kubernetes') {
            when { expression { env.BRANCH_NAME != 'main'} }
            steps {
                script {
                    withKubeConfig(credentialsId: "kubeconfig") {
                        sh('''
                            cat k8s/testing-api-deploy.yaml | envsubst | kubectl apply -f -kubectl apply -f k8s/testing-api-svc.yaml
                            sleep 10
                            echo "Deploy Version:${VERSION}"
                        ''')
                    }
                }
            }
        }
    }
}
