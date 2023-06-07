pipeline {
    agent {
            docker { 
                image 'wyleung/python-builder:latest'
                args '-u root:root'
            }
        }
    stages {
        stage('Before Install') {
            steps { 
                sh 'python --version && pip --version'
                sh 'g++ --version '
            }
        }
        stage('Install Clang') {
            steps { 
                sh 'apt update'
                sh 'apt install -y clang'
                sh 'clang++ --version'
            }
        }
        stage('Install PlatformIO') {
            steps {
                sh 'pip install --user --upgrade pip'
                sh 'pip install --user --upgrade platformio'
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
