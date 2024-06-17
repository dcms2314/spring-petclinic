pipeline {
    agent any
    
    tools {
        jdk 'JDK17'
        maven 'M3'
    }
    
    stages {
        stage('Git Clone') {
            steps {
                echo 'Git Clone'
                git url: 'https://github.com/dcms2314/spring-petclinic.git',
                branch: 'efficient-webjars'
            }
            post {
                success {
                    echo 'Success git clone'
                }
                failure {
                    echo 'Fail git clone step'
                }
            }
        }    
        stage('Maven Build') {
            steps {
                echo 'Maven Build'
                sh 'mvn -Dmaven.test.failure.ignore=true install'
            }
            post {
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                }
            }
        }
        stage('Docker Image Upload') {
            steps {
                echo 'Docker Image Upload'
                dir("${env.WORKSPACE}") {
                    sh """
                        docker build -t dcms2314/spring-petclinic:$BUILD_NUMBER .
                        docker tag dcms2314/spring-petclinic:$BUILD_NUMBER dcms2314/spring-petclinic:latest
                    """
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploy'
            }
        }
    }
}
