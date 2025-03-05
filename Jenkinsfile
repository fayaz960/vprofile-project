pipeline {
    agent any
    
    tools {
        maven "MAVEN3.9"
        jdk "JDK17"
    }

    environment {
        SNAP_REPO = 'vprofile-snapshot'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin123'
        RELEASE_REPO = 'vprofile-release'
        CENTRAL_REPO = 'vpro-maven-central'
        NEXUSIP = '172.31.35.75'
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'vpro-maven-group'
        NEXUS_LOGIN = 'nexuslogin'
    }

    stages {
        stage('Build') {
            steps {
                echo "Building the project..."
                sh 'mvn -s settings.xml -DskipTests install'
            }
            post {
                success {
                    echo "Build completed successfully. Now archiving artifacts."
                    archiveArtifacts artifacts: '**/*.war'
                }
                failure {
                    echo "Build failed! Check the logs."
                }
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh 'mvn test'
            }
            post {
                success {
                    echo "All tests passed successfully."
                }
                failure {
                    echo "Some tests failed! Please review the test results."
                }
            }
        }

        stage('Checkstyle Analysis') {
            steps {
                echo "Running Checkstyle analysis..."
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            }
            post {
                success {
                    echo "Checkstyle passed without issues."
                }
                failure {
                    echo "Checkstyle detected issues! Please review and fix them."
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline execution finished."
        }
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline encountered errors. Please check logs."
        }
    }
}
