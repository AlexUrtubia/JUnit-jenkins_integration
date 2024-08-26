pipeline {
    agent any

    tools {
        maven "maven-integration"
        jdk "jdk-17"
    }

    environment {
        // Cargar el token de Slack desde las credenciales
        SLACK_TOKEN = credentials('slackToken')
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Clonando el repositorio...'
                git branch: 'main', url: 'https://github.com/AlexUrtubia/JUnit-jenkins_integration'
            }
        }

        stage('Build') {
            steps {
                echo 'Compilando el proyecto...'
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Ejecutando pruebas unitarias...'
                sh 'mvn test'
            }
            post {
                // Publicar resultados de las pruebas JUnit
                junit '**/target/surefire-reports/*.xml'
            }
        }

        stage('Coverage') {
            steps {
                echo 'Generando informe de cobertura...'
                sh 'mvn cobertura:cobertura'
            }
            post {
                // Publicar el informe de cobertura
                cobertura coberturaReportFile: '**/target/site/cobertura/coverage.xml'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finalizado.'
        }
        success {
            echo 'Compilación exitosa.'
            slackSend (
                channel: 'D07DYDV69V1',
                color: 'good',
                tokenCredentialId: 'slackToken',
                message: "*${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}*\nMás información en: ${env.BUILD_URL}"
            )
        }
        failure {
            echo 'Falló la compilación.'
            slackSend (
                channel: 'D07DYDV69V1',
                color: 'danger',
                tokenCredentialId: 'slackToken',
                message: "*${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}*\nMás información en: ${env.BUILD_URL}"
            )
        }
    }
}
