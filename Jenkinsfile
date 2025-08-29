pipeline {
    agent any

    environment {
        // Jenkins credential ID for your SonarQube token
        SONAR_TOKEN = credentials('workshop_token')
    }
    stages {
        stage('SonarQube Analysis') {
            steps {
                script {
                    echo 'Starting SonarQube code analysis...'
                }
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    withSonarQubeEnv('SonarQube') {
                        // If using a sonar-project.properties file, you can remove -D options
                        sh """
                            ${tool 'SonarScanner'}/bin/sonar-scanner \
                            -Dsonar.projectKey=workshop1 \
                            -Dsonar.sources=. \
                            -Dsonar.login=$SONAR_TOKEN
                        """
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
        success {
            echo 'Code passed SonarQube Quality Gate!'
        }
        failure {
            echo 'Pipeline failed. Check logs and SonarQube analysis results.'
        }
    }
}
