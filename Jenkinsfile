pipeline {
    agent any
   
    stages {
        stage('Checkout') {
            steps {
                // Checkout the repository
                checkout scm
            }
        }

        stage('Run Tests') {
            
            when {
                // Run tests only on main branch for CodeCoverage
                branch 'main'
                // Run the CodeCoverage Test only on the MAIN Branch.
                expression {
                    return env.BRANCH_NAME == 'main'
                }
              }
            
            steps {
                script {
                    // Example command to run CodeCoverage tests
                    sh 'mvn clean test -P CodeCoverage'
                    //Running CodeCoverage Test Here
                    sh 'echo "Running CodeCoverage test"'
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
                    //docker.withRegistry('https://your.docker.registry.url', 'credentials-id') {
                        docker.withRegistry('https://hub.docker.com/repositories/limacadmin', 'limacadmin') {
                        docker.image("repository/${imageName}:${version}").push()
                        }
                }
            }
        }
    }
}
