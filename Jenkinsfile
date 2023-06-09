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
    triggers {
        pollSCM('H/5 * * * *')
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
    }
    post { 
        success { 
            sh 'cat ./Testing/Temporary/LastTest.log'
        }
        failure { 
            sh 'cat ./Testing/Temporary/LastTestFailures.log'
        }
        always { 
            sh 'cat ./Testing/Temporary/CTestCostData.txt'
            emailtext subject: 'Job \'${JOB_NAME}\' (${BUILD_NUMBER}) has changed status',
            body: 'Please go to ${BUILD_URL} and verify the build',
            attachLog: true,
            compressLog: true,
            to: 'test@jenkins',
            recipientProviders: [upstreamDevelopers(), requestor()]]  
        }
        changed {
            emailtext subject: 'Job \'${JOB_NAME}\' (${BUILD_NUMBER}) has changed status',
                body: 'Please go to ${BUILD_URL} and verify the build',
                attachLog: true,
                compressLog: true,
                to: 'test@jenkins',
                recipientProviders: [upstreamDevelopers(), requestor()]]  
        }
    }
}
