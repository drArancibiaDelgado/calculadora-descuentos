// Practica 6 - Pipeline de calculadora-descuentos
pipeline {
    agent any

    tools {
        maven 'Maven-3.9'
    }

    options {
        timestamps()
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timeout(time: 15, unit: 'MINUTES')
    }

    triggers {
        pollSCM('H/2 * * * *')
    }

    stages {
        stage('Compilar') {
            steps {
                sh 'mvn -B clean compile'
            }
        }
        
        stage('Pruebas') {
            steps {
                // Sin el failure.ignore para que respete el Defecto 4
                sh 'mvn -B test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        
        // La nueva etapa va completamente separada de las demás
        stage('Cobertura') {
            steps {
                sh 'mvn -B verify'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/site/jacoco/**/*', fingerprint: true
                }
            }
        }
        
        stage('Empaquetar') {
            steps {
                sh 'mvn -B -DskipTests package'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Build y pruebas exitosas'
        }
        failure {
            echo 'Fallo el proceso: revisa la etapa en rojo'
        }
    }
}