pipeline {
    agent { label 'Java_Env' }

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-21-openjdk-amd64'
        PATH = "/usr/lib/jvm/java-21-openjdk-amd64/bin:${env.PATH}"
    }


    stages {

        stage('Verify Java Toolchain') {
            steps {
                sh '''
                    echo "JAVA_HOME=$JAVA_HOME"
                    echo "PATH=$PATH"
                    ls -l $JAVA_HOME/bin/java
                    ls -l $JAVA_HOME/bin/javac
                    java -version
                    javac -version
                    mvn -version
                '''
            }
        }


        stage('Checkout') {
            steps {
                cleanWs()
                git branch: 'feature-1',
                    url: 'https://github.com/Mallesha3/bus_booking.git'
            }
        }

        stage('Build') {
            steps {
                sh '''
                    mvn clean install
                '''
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'jfrog',
                    usernameVariable: 'JFROG_USER',
                    passwordVariable: 'JFROG_API_KEY'
                )]) {
                    sh '''
                        mvn deploy
                    '''
                }
            }
        }
    }
}
