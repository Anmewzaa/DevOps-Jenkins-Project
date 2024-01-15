pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main'], [name: '*/dev']], extensions: [], userRemoteConfigs: [[credentialsId: 'Github-Token', url: 'https://github.com/Anmewzaa/FutureSkill-DevOps-Course.git']])
            }
        }
    }
}
