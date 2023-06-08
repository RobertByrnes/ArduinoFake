pipeline { 
    agent {
            docker { 
                image 'robertbyrnes1978/platformio-pipeline'
                args '-u root:root'
                // registryUrl 'https://registry.hub.docker.com/v2/'
                registryUrl 'https://index.docker.io/v2/'
                // registryCredentialsId 'b44e395a-46d5-4683-a237-91e11f269b6b'
            }
        }
    stages {
        stage('Before Install') {
            steps { 
                sh 'python --version'
                sh 'pip --version'
                sh 'g++ --version'
                sh 'clang++ --version'
                sh 'git config --global --add safe.directory /var/jenkins_home/workspace/ArduinoFake'
            }
        }
        stage('Run CMake') {
            steps {
                sh 'cmake .'
            }
        }
        stage('Run Make') {
            steps {
                sh 'make'
            }
        }
        stage('Unit Tests') {
            steps {
                sh 'make test'
            }
        }
        stage('After Unit Tests') {
            steps {
                sh 'ls ./Testing/Temporary'
            }
        }
    }
}
