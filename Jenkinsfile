pipeline {
  agent {
    node {
      label 'ibmi'
    }
  }

  environment {
//     OBJECT_MODE = '64'
//     CC = 'gcc'
//     CXX = 'g++'
    CFLAGS="-DPASE"
  }

  stages {
    stage('setup') {
      steps {
        dir("IBM_DB/ibm_db") {
          sh 'python3 -m venv --clear .venv'
        }
      }
    }

    stage('build') {
      steps {
        dir("IBM_DB/ibm_db") {
          sh '. .venv/bin/activate; pip install .'
        }
      }
    }

    stage('test') {
      environment {
        DATABASE = credentials('database-creds')
      }
      steps {
        dir("IBM_DB/ibm_db") {
          sh label: 'setting up config', script:"perl -p -e \"s|sample|*LOCAL|; s|db2inst1|$DATABASE_USR|; s|'password'|'$DATABASE_PSW'|; \" config.py.sample > config.py"
          sh '. .venv/bin/activate; python tests.py'
        }
      }
    }
  }
}
