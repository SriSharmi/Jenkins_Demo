
pipeline {
    agent any

    options {
        timestamps()
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Getting source code from GitHub'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Checking HTML website files'

                bat '''
                    if not exist index.html (
                        echo ERROR: index.html not found
                        exit /b 1
                    )
                '''

                echo 'Build completed successfully'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing HTML website'

                bat '''
                    findstr /C:"Welcome to Jenkins CI/CD!" index.html

                    if errorlevel 1 (
                        echo ERROR: Required website content not found
                        exit /b 1
                    )
                '''

                echo 'Website test passed'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying website to Windows'

                bat '''
                    if not exist "%PUBLIC%\\JenkinsDeploy" (
                        mkdir "%PUBLIC%\\JenkinsDeploy"
                    )

                    copy /Y index.html "%PUBLIC%\\JenkinsDeploy\\index.html"

                    if errorlevel 1 exit /b 1
                '''

                echo 'Website deployed successfully'
            }
        }
    }

    post {
        success {
            echo 'CI/CD PIPELINE COMPLETED SUCCESSFULLY!'
        }

        failure {
            echo 'CI/CD PIPELINE FAILED. Check Console Output.'
        }
    }
}
