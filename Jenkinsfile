pipeline {
    agent {
            docker { 
                image 'platformio:latest'
                args '-u root:root'
            }
        }
    stages {
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
                        values 'g++-10.2.1', 'clang++-11.0.1-2'
                    }
                }

                stages {
                    stage('GCC Build') {
                        when {
                            expression {
                                params.COMPILER == 'g++-10.2.1'
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
                                params.COMPILER == 'clang++-11.0.1-2'
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
