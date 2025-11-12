pipeline {
    agent any

    environment {
        // Nombre del servidor SonarQube configurado en Jenkins
        SONARQUBE_SERVER = 'SonarQube'
        SONAR_HOST_URL = 'http://172.18.0.3:9000'
        SONAR_AUTH_TOKEN = credentials('SonarQubeToken')
        // Agregar sonar-scanner al PATH
        PATH = "/opt/sonar-scanner/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                // Clonar el código fuente desde el repositorio
                checkout scm
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    // Usar la instalación de SonarScanner configurada en Jenkins
                    script {
                        def scannerHome = tool 'SonarQubeScanner' // nombre de la instalación del scanner
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=DVWA \
                            -Dsonar.sources=vulnerabilities \
                            -Dsonar.php.version=8.0
                        """
                    }
                }
            }
        }
        // stage('Quality Gate') {
        //     steps {
        //         // Esperar el resultado del Quality Gate
        //         timeout(time: 1, unit: 'HOURS') {
        //             waitForQualityGate abortPipeline: true
        //         }
        //     }
        // }
    }
}
