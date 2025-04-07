pipeline {
    agent any

    triggers {
        githubPush() // GitHub'dan yapılan push işlemi tetikleyicisi
    }

    environment {
        IMAGE_NAME = "html-sayfam"
        CONTAINER_NAME = "html-sayfam-container"
        PATH = "C:\\Program Files\\Docker\\Docker\\resources\\bin;${env.PATH}"
    }

    stages {
        stage('Klonla') {
            steps {
                echo 'GitHub reposu klonlanıyor...'
                git branch: 'main', url: 'https://github.com/imranbeyza/YMG2V-ZE.git'
            }
        }

        stage('Docker Image Oluştur') {
            steps {
                echo 'Docker image oluşturuluyor...'
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Eski Containerı Durdur ve Sil') {
            steps {
                script {
                    echo 'Eski container kontrol ediliyor...'
                    bat """
                        for /f "tokens=*" %%i in ('docker ps -aq -f name=%CONTAINER_NAME%') do (
                            docker stop %%i
                            docker rm %%i
                        )
                    """
                }
            }
        }

        stage('Yeni Container Oluştur') {
            steps {
                echo 'Yeni container başlatılıyor...'
                bat 'docker run -d -p 8080:80 --name %CONTAINER_NAME% %IMAGE_NAME%'
            }
        }
    }

    post {
        success {
            echo 'Pipeline başarıyla tamamlandı ✅'
        }
        failure {
            echo 'Bir hata oluştu ❌'
        }
        always {
            echo 'Pipeline tamamlandı (başarılı veya başarısız)'
        }
    }
}
