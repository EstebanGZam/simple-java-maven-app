pipeline {
    agent {
        docker {
            image 'maven:3.9.2'
            args '-u root'
        }
    }
    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'master', description: 'Especifique la rama a compilar (ej: master, declarative, testng)')
    }
    stages {
        stage('Prepare Workspace') {
            steps {
                script {
                    if (fileExists('simple-java-maven-app')) {
                        echo "Eliminando carpeta existente..."
                        sh 'rm -rf simple-java-maven-app'
                    }
                }
            }
        }
        stage('Checkout') {
            steps {
                script {
                    echo "Clonando rama seleccionada: ${params.BRANCH_NAME}"
                }
                git branch: "${params.BRANCH_NAME}", url: 'https://github.com/EstebanGZam/simple-java-maven-app.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
