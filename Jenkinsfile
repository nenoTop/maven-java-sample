pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Maven Compile'
                sh 'mvn compile'
            }
        }
        stage('Test') {
            steps {
                echo 'Maven Test'
                sh 'mvn test'
            }
        }
        stage('Package') {
            steps {
                echo 'Maven Package'
                sh 'mvn package'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
                success {
                    archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
                }
            }

        }
        stage('Deploy') {
            steps {
                echo 'Deploy'
                withEnv(['JENKINS_NODE_COOKIE=dontKillMe']) {
                    sh 'nohup java -jar -Dserver.port=8082 target/spring-petclinic-2.3.1.BUILD-SNAPSHOT.jar &'
                }
            }
        }

    }
}
