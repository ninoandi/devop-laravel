node {
    checkout scm
    
    stage("Build") {
        sh '''
            # PAKSA menggunakan socket lokal dengan flag -H
            # Abaikan semua environment variable dan konfigurasi
            DOCKER_CMD="docker -H unix:///var/run/docker.sock"
            
            echo "=== TEST KONEKSI DOCKER ==="
            $DOCKER_CMD version
            
            echo "=== PULL IMAGE PHP 8.2 ==="
            $DOCKER_CMD pull shippingdocker/php-composer:8.2
            
            echo "=== JALANKAN COMPOSER ==="
            $DOCKER_CMD run --rm \\
                -u root \\
                -v $(pwd):/app \\
                -w /app \\
                shippingdocker/php-composer:8.2 \\
                sh -c "rm -f composer.lock && composer install --no-interaction"
        '''
    }
    
    stage("Test") {
        sh '''
            docker -H unix:///var/run/docker.sock run --rm ubuntu:22.04 echo "TEST BERHASIL!"
        '''
    }
}