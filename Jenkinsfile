// Practica 6 - Pipeline de calculadora-descuentos
pipeline {
    agent any

    tools {
        maven 'Maven 3.8.5'
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
                bat 'mvn -B clean compile'
            }
        }
        stage('Pruebas') {
            steps {
                bat 'mvn -B test -Dmaven.test.failure.ignore=true'
            }
        }
        stage('Empaquetar') {
            steps {
                bat 'mvn -B -DskipTests package'
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
