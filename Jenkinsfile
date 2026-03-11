node {
    checkout scm
    
    // deploy env dev
    stage("Build") {
        // Reset DOCKER_HOST ke socket lokal
        sh '''
            unset DOCKER_HOST
            export DOCKER_HOST=unix:///var/run/docker.sock
            echo "DOCKER_HOST: $DOCKER_HOST"
        '''
        
        // Pastikan Docker tersedia
        sh 'docker --version'
        
        // Pull image terlebih dahulu
        sh 'docker pull shippingdocker/php-composer:8.2 || true'
        
        // HAPUS volume mount dari inside() karena sudah di-mount di level container
        docker.image('shippingdocker/php-composer:8.2').inside('-u root') {
            sh '''
                echo "Inside composer container..."
                rm -f composer.lock
                composer install --no-interaction
            '''
        }
    }
    
    // Testing
    stage("Test") {
        // Reset DOCKER_HOST lagi
        sh 'unset DOCKER_HOST'
        
        // HAPUS volume mount dari inside()
        docker.image('ubuntu:22.04').inside('-u root') {
            sh '''
                echo "Inside ubuntu container..."
                echo "Ini adalah test"
                cat /etc/os-release
            '''
        }
    }
}