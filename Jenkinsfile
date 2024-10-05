pipeline {
    agent {
        label "app_builder_android"
    }

	environment {
		FLUTTER_HOME = '/home/agustine/development/flutter'
		ANDROID_HOME = '/home/agustine/Android/Sdk'
		PATH="${FLUTTER_HOME}/bin:/bin:${PATH}"
	}

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
		dir('material_3_demo') {
                	sh 'flutter pub get'
		}
            }
        }

        stage('Run Tests') {
            steps {
		dir('material_3_demo') {
                	sh 'flutter test'
		}
            }
        }

        stage('Build APK') {
            steps {
		dir('material_3_demo') {
                	sh 'flutter build apk --release'
		}
            }
        }

        stage('Archive APK') {
            steps {
                archiveArtifacts artifacts: '**/build/app/outputs/flutter-apk/app-release.apk', fingerprint: true
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
