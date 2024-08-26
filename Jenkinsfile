pipeline {
    agent any

    tools {
        maven "maven-integration"
        jdk "jdk-17"
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
        }

        stage('Code Coverage Report') {
            steps {
                echo 'Generando informe de cobertura de código...'
                sh 'mvn jacoco:report'
            }
            post {
                always {
                    echo 'Publicando informe de cobertura...'
                    jacoco execPattern: '**/target/jacoco.exec'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finalizado.'
        }
        success {
            echo 'Compilación exitosa.'
        }
        failure {
            echo 'Falló la compilación.'
        }
    }
}
