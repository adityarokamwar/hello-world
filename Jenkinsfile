pipeline {

    agent any



    environment {

        MAVEN_HOME = '/usr/share/maven' // Path to Maven installation
        // Use the exact server name you configured in Jenkins
        SONARQUBE_ENV = 'Local SonarQube'  
        // Name of the scanner tool configured in Jenkins
        SONAR_SCANNER_INSTALL = 'SonarQube Scanner 4.7+'
        // Credentials ID for your SonarQube token in Jenkins
        SONAR_TOKEN_CREDENTIAL_ID = 'sonar-token'
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

        stage('Run SonarQube Analysis') {
            steps {
                // Use the correct environment and tools
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    script {
                        def scannerHome = tool 'SonarQube Scanner 4.7+'
                        sh """
                          ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=scmgalaxy1 \
                            -Dsonar.sources=server/src \
                            -Dsonar.host.url=${SONAR_HOST_URL} \
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
