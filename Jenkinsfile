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
                git 'https://github.com/kullaniciAdi/repo-adi.git'
            }
        }

        stage('Docker Image Oluştur') {
            steps {
                echo 'Docker image oluşturuluyor...'
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Eski Containerı Durdur ve Sil') {
            steps {
                script {
                    echo 'Eski container kontrol ediliyor...'
                    sh """
                        if [ \$(docker ps -aq -f name=$CONTAINER_NAME) ]; then
                            docker stop $CONTAINER_NAME || true
                            docker rm $CONTAINER_NAME || true
                        fi
                    """
                }
            }
        }

        stage('Yeni Container Oluştur') {
            steps {
                echo 'Yeni container başlatılıyor...'
                sh 'docker run -d -p 8080:80 --name $CONTAINER_NAME $IMAGE_NAME'
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
