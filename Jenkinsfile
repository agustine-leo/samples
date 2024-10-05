pipeline {
    agent none

    stages {
        stage('Build application') {
            parallel {
                stage('Android Builder') {
                    agent {
                        label "app_builder_android"
                    }

                    stages {
                        stage('Checkout') {
                            steps {
                                checkout scm
                            }
                        }

                        stage('Install Dependencies') {
                            steps {
                                sh 'flutter pub get'
                            }
                        }

                        stage('Run Tests') {
                            steps {
                                sh 'flutter test'
                            }
                        }

                        stage('Build APK') {
                            steps {
                                sh 'flutter build apk --release'
                            }
                        }

                        stage('Archive APK') {
                            steps {
                                archiveArtifacts artifacts: '**/build/app/outputs/flutter-apk/app-release.apk', fingerprint: true
                            }
                        }
                    }
                }

                stage('Linux Builder') {
                    agent {
                        label "app_builder_android"
                    }

                    stages {
                        stage('Checkout') {
                            steps {
                                checkout scm
                            }
                        }

                        stage('Install Dependencies') {
                            steps {
                                sh 'flutter pub get'
                            }
                        }

                        stage('Run Tests') {
                            steps {
                                sh 'flutter test'
                            }
                        }

                        stage('Build Linux') {
                            steps {
                                sh 'flutter build linux --release'
                            }
                        }

                        stage('Archive Linux') {
                            steps {
                                archiveArtifacts artifacts: '**/build/linux/x64/release/bundle/material_3_demo', fingerprint: true
                            }
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
