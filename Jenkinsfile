pipeline {
    agent {
            docker { 
                image 'shaguarger/platformio:latest'
                args '-u root:root'
            }
        }
    stages {
        stage('Install Clang++') {
            steps {
                sh 'apt-get update'
                sh 'apt-get install -y clang-3.6'
                sh 'update-alternatives --install /usr/bin/clang++ clang++ /usr/bin/clang++-3.6 100'
            }
        }
        stage('Before Install') {
            steps { 
                sh 'python --version'
                sh 'pip --version'
                sh 'g++ --version'
                sh 'clang++ --version'
            }
        }
        stage('Build') {
            matrix {
                axes {
                    axis {
                        name 'COMPILER'
                        values 'g++-6.4.0', 'clang++-'
                    }
                }

                stages {
                    stage('GCC Build') {
                        when {
                            expression {
                                params.COMPILER == 'g++-6.4.0'
                            }
                        }
                        steps {
                            script {
                                env.COMPILER = params.COMPILER
                                env.MAKE_TASK = 'cmake-test'
                            }
                            sh 'make $MAKE_TASK'
                        }
                    }

                    stage('Clang Build') {
                        when {
                            expression {
                                params.COMPILER == 'clang++-3.6'
                            }
                        }
                        steps {
                            script {
                                env.COMPILER = params.COMPILER
                                env.MAKE_TASK = 'cmake-test'
                            }
                            sh 'make $MAKE_TASK'
                        }
                    }
                }
            }
        }
    }
}
