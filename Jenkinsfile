pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver for development') {
            agent {
                docker {
                    image 'cdrx/pyinstaller-linux:python2'
                }
            }
            when {
                branch 'development'
            }
            steps {
                sh 'pyinstaller --onefile sources/add2vals.py'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
        stage('Deploy for production') {
            agent {
                docker {
                    image 'cdrx/pyinstaller-linux:python2'
                }
            }
            when {
                branch 'production'
            }
            steps {
                sh 'pyinstaller --onefile sources/add2vals.py'
                input message: 'Finished using the web site? (Click "Proceed" to continue'
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
    }
}