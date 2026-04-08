pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                echo 'Code already checked out from Git'
            }
        }

        stage('Compile Java') {
            steps {
                sh '''
                rm -rf build
                mkdir build
                javac -d build src/Hello.java
                jar cfe hello.jar Hello -C build .
                '''
            }
        }

        stage('Prepare Package Directory') {
            steps {
                sh '''
                rm -rf package
                mkdir -p package/usr/local/bin
                cp hello.jar package/usr/local/bin/
                '''
            }
        }

        stage('Build DEB using FPM') {
            steps {
                sh '''
                fpm -s dir -t deb -n hello-java -v 1.0.${BUILD_NUMBER} --prefix=/ -C package usr
                '''
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: '*.deb'
        }
    }
}
