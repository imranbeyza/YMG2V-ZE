pipeline {
    agent any

    environment {
        IMAGE_NAME = "html-sayfam"
        CONTAINER_NAME = "html-sayfam-container"
    }

    stages {
        stage('Klonla') {
            steps {
                echo 'GitHub reposu klonlanıyor...'
                git 'https://github.com/imranbeyza/YMG2V-ZE.git'
            }
        }

        stage('Docker Image Oluştur') {
            steps {
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Eski Containerı Durdur ve Sil') {
            steps {
                bat '''
                docker stop %CONTAINER_NAME% || exit 0
                docker rm %CONTAINER_NAME% || exit 0
                '''
            }
        }

        stage('Yeni Container Oluştur') {
            steps {
                bat 'docker run -d -p 8080:80 --name %CONTAINER_NAME% %IMAGE_NAME%'
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline başarıyla tamamlandı'
        }
        failure {
            echo '❌ Bir hata oluştu'
        }
        always {
            echo 'ℹ️ Pipeline tamamlandı (başarılı veya başarısız)'
        }
    }
}
