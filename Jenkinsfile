pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Run Tests') {
            when {
                expression { env.BRANCH_NAME == 'main' }
            }
            steps {
                script {
                    sh 'mvn clean test -P CodeCoverage'
                }
            }
        }
        
        stage('Build Container') {
            when {
                expression { env.BRANCH_NAME != 'playground' }
            }
            steps {
                script {
                    def imageName
                    def imageVersion
                    
                    if (env.BRANCH_NAME == 'main') {
                        imageName = 'calculator'
                        imageVersion = '1.0'
                    } else {
                        imageName = 'calculator-feature'
                        imageVersion = '0.1'
                    }
                    
                    docker.build("repository/${imageName}:${imageVersion}")
                }
            }
        }
        
        stage('Push Container') {
            when {
                allOf {
                    expression { env.BRANCH_NAME != 'playground' }
                    expression { currentBuild.result == 'SUCCESS' }
                }
            }
            steps {
                script {
                    docker.withRegistry('https://hub.docker.com', 'docker-credentials') {
                        def imageName
                        def imageVersion
                        
                        if (env.BRANCH_NAME == 'main') {
                            imageName = 'calculator'
                            imageVersion = '1.0'
                        } else {
                            imageName = 'calculator-feature'
                            imageVersion = '0.1'
                        }
                        
                        docker.image("repository/${imageName}:${imageVersion}").push()
                    }
                }
            }
        }
    }
}
