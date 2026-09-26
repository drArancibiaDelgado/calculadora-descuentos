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
                sh 'mvn -B test -Dmaven.test.failure.ignore=true'
            }
            post {
        always {
            junit 'target/surefire-reports/*.xml'
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
