pipeline {

    agent any



    environment {

        MAVEN_HOME = '/usr/share/maven' // Path to Maven installation
        SONARQUBE_SERVER = 'Local SonarQube' // Name in Jenkins config
        SONAR_SCANNER = 'SonarQube Scanner 4.7+' // Name of the installed scanner
        SONAR_TOKEN = credentials('sonar-token') // Jenkins Credentials ID
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

        stage('Run SonarQube Scanner') {
            environment {
                scannerHome = "${SONAR_SCANNER_INSTALL}"
            }
            steps {
                // Use Jenkins' withSonarQubeEnv to set environment variables
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    sh '''
                        ${scannerHome}/bin/sonar-scanner \
                          -Dsonar.projectKey=scmgalaxy1 \
                          -Dsonar.sources=server/src \
                          -Dsonar.host.url=${SONAR_HOST_URL} \
                          -Dsonar.login=${SONAR_TOKEN}
                    '''
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
