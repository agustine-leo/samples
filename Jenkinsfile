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
                            buildFlutterApp('apk', '**/build/app/outputs/flutter-apk/app-release.apk', 'app_builder_android')
                        }
                    }
                }

                stage('Linux Builder') {
                    steps {
                        script {
                            buildFlutterApp('linux', '**/build/linux/x64/release/bundle/material_3_demo', 'app_builder_android')
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


def buildFlutterApp(target, artifactPath, agentLabel) {
    stage('Checkout') {
        agent { label agentLabel }
        checkout scm
    }

    stage('Install Dependencies') {
        agent { label agentLabel }
        sh 'flutter pub get'
    }

    stage('Run Tests') {
        agent { label agentLabel }
        sh 'flutter test'
    }

    stage('Build App') {
        agent { label agentLabel }
        sh "flutter build ${target} --release"
    }

    stage('Archive Artifact') {
        agent { label agentLabel }
        archiveArtifacts artifacts: artifactPath, fingerprint: true
    }
}
