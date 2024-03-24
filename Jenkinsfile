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
                // Run tests only on main branch for CodeCoverage
                branch 'main'
            }
            steps {
                script {
                    sh 'mvn clean test' // Example Maven command for tests
                }
            }
        }

        stage('Build Container') {
            when {
                // Build container only for main and feature branches
                anyOf {
                    branch 'main'
                    branch 'feature'
                }
            }
            steps {
                script {
                    // Determine image name and version based on branch
                    def imageName = ''
                    def version = ''
                    if (env.BRANCH_NAME == 'main') {
                        imageName = 'calculator'
                        version = '1.0'
                    } else if (env.BRANCH_NAME == 'feature') {
                        imageName = 'calculator-feature'
                        version = '0.1'
                    }
                    
                    // Build and push container if tests succeed
                    docker.build("repository/${imageName}:${version}")
                    docker.withRegistry('https://your.docker.registry.url', 'credentials-id') {
                        docker.image("repository/${imageName}:${version}").push()
                    }
                }
            }
        }
    }
}
