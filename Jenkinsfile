node {
    checkout scm

    // deploy env dev
    stage("Build"){
        withEnv(['DOCKER_HOST=tcp://172.29.115.193']) {
            docker.image('php:8.2-cli').inside('-u root') {
                sh '''
                    apt-get update && apt-get install -y libzip-dev libpng-dev
                    docker-php-ext-install gd zip bcmath
                    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
                    rm -f composer.lock
                    composer install
                '''
            }
        }
    }

    // Testing
    stage("Testing"){
        withEnv(['DOCKER_HOST=tcp://172.24.96.1:2375']) {
            docker.image('ubuntu:22.04').inside('-u root') {
                sh 'echo "Ini adalah test"'
            }
        }
    }
}