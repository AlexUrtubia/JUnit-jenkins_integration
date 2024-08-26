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
                always {
                    // Publicar resultados de las pruebas JUnit
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Coverage') {
            steps {
                echo 'Generando informe de cobertura...'
                sh 'mvn cobertura:cobertura'
            }
            post {
                always {
                    // Publicar el informe de cobertura
                    cobertura coberturaReportFile: '**/target/site/cobertura/coverage.xml'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finalizado.'
            slackSend (
                channel: '#general', // Reemplaza con tu canal de Slack
                color: currentBuild.currentResult == 'SUCCESS' ? 'good' : 'danger',
                tokenCredentialId: 'slackToken',
                message: "*${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}*\nMás información en: ${env.BUILD_URL}"
            )
        }
        success {
            echo 'Compilación exitosa.'
        }
        failure {
            echo 'Falló la compilación.'
        }
    }
}
