node {
    stage('Build') {
        docker.image('python:2-alpine') {
            checkout scm
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    stage('Test') {
        docker.image('qnib/pytest') {
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
    stage('Deliver') {
        docker.image('cdrx/pyinstaller-linux:python2') {
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