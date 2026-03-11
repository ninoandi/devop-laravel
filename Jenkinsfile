node {
    checkout scm
    
    // deploy env dev
    stage("Build") {
        // Pastikan Docker tersedia
        sh 'docker --version || echo "Docker tidak tersedia"'
        
        // Pull image terlebih dahulu
        sh 'docker pull shippingdocker/php-composer:7.4 || echo "Gagal pull image"'
        
        // Gunakan inside dengan volume mount
        docker.image('shippingdocker/php-composer:7.4').inside('-u root -v /var/run/docker.sock:/var/run/docker.sock') {
            sh 'rm -f composer.lock'
            sh 'composer install --no-interaction'
        }
    }
    
    // Testing
    stage("Test") {
        docker.image('ubuntu:22.04').inside('-u root -v /var/run/docker.sock:/var/run/docker.sock') {
            sh 'echo "Ini adalah test"'
            sh 'cat /etc/os-release'
        }
    }
}