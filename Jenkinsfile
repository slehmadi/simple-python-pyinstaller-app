node {
    docker.image('python:2-alpine') {
        stage('Build') {
            checkout scm
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    docker.image('qnib/pytest') {
        stage('Test') {
            checkout scm
            try {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            } catch(e) {
                echo 'error'
            } finally {
                junit 'test-reports/results.xml'
            }
        }
    }
    docker.image('cdrx/pyinstaller-linux:python2') {
        stage('Deliver') {
            checkout scm
            try {
                sh 'pyinstaller --onefile sources/add2vals.py'
                archiveArtifacts 'dist/add2vals'
            } catch(e) {
                echo 'error'
            }
        }
    }
}