pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                script {
                    // Ejecuta el build con Maven
                    sh 'mvn clean install'
                }
            }
        }
        
        stage('Coverage') {
            steps {
                script {
                    echo 'Generando informe de cobertura...'
                    sh 'mvn jacoco:report'
                }
            }
        }
        
        stage('Publish Coverage Report') {
            steps {
                script {
                    // Publica el informe de cobertura
                    jacoco execPattern: '**/target/*.exec', classPattern: '**/target/classes', sourcePattern: '**/src/main/java', inclusionPattern: '**/target/classes/**/*.class'
                }
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline finalizado.'
            slackSend channel: 'D07DYDV69V1', color: 'good', message: 'La compilación y los informes de cobertura se completaron exitosamente.'
        }
    }
}
