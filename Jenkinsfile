pipeline {

    agent any



    environment {

        MAVEN_HOME = '/usr/share/maven' // Path to Maven installation
        SONARQUBE_SERVER = 'Local SonarQube'
        // Scanner tool name (must match Jenkins Global Tools configuration)
        SCANNER_NAME = 'SonarQube Scanner 4.7+'
        // Credential ID in Jenkins for SonarQube token
        SONAR_TOKEN_CREDENTIAL_ID = 'sonar-token'
        // Your actual SonarQube server URL
        SONAR_HOST_URL = 'http://13.239.36.249:9000'

    }



    stages {

        stage('Checkout Code') { // Clones the repository

            steps {

                git 'https://github.com/adityarokamwar/hello-world.git'

            }

        }



        stage('Build with Maven') { // Builds the project and creates JAR/WAR

            steps {

                sh 'mvn clean package'

            }

        }



        stage('Run Unit Tests') { // Executes unit tests

            steps {

                sh 'mvn test'

            }

        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv(env.SONARQUBE_SERVER) {
                    script {
                        def scannerHome = tool(env.SCANNER_NAME)

                        sh """
                          ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=scmgalaxy1 \
                            -Dsonar.sources=server/src \
                            -Dsonar.host.url=${env.SONAR_HOST_URL} \
                            -Dsonar.login=${env.SONAR_TOKEN}
                        """
                    }
                }
            }
        }

    }



    post {

        success {

            echo 'Build and deployment successful!'

        }

        failure {

            echo 'Build failed!'

        }

    }

}
