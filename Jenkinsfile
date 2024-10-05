pipeline {
    agent none
    environment {
	FLUTTER_HOME = '/home/agustine/development/flutter'
	ANDROID_HOME = '/home/agustine/Android/Sdk'
	PATH="${FLUTTER_HOME}/bin:/bin:${PATH}"
    }

    stages {
        stage('Build application') {
            parallel {
                stage('Android Builder') {
                    agent { label 'app_builder_android' }

                    steps {
                        script {
                            buildFlutterApp('apk', '**/build/app/outputs/flutter-apk/app-release.apk')
                        }
                    }
                }

                stage('Linux Builder') {
                    agent { label 'app_builder_android' }

                    steps {
                        script {
                            buildFlutterApp('linux', '**/build/linux/x64/release/bundle/material_3_demo')
                        }
                    }
                }
            }
        }
    }
    

    post {
        always {
            cleanWs()
        }
    }
}


def buildFlutterApp(target, artifactPath) {
    stage('Checkout') {
        checkout scm
    }

    stage('Install Dependencies') {
        sh 'flutter pub get'
    }

    stage('Run Tests') {
        sh 'flutter test'
    }

    stage('Build App') {
        sh "flutter build ${target} --release"
    }

    stage('Archive Artifact') {
        archiveArtifacts artifacts: artifactPath, fingerprint: true
    }
}
