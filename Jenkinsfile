pipeline {
    
    agent any 
    stages {
        stage('Checkout') {
            steps {
                echo 'Getting source  code  form GitHub'
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                sh '''
                  python3 -m venv .venv
                  .venv/bin/python -m pip install -r requirements.txt
                  '''
            }
        }
        stage('Unit Tests') {
            steps {
                sh '''
                  mkdir -p reports
                  
                  .venv/bin/python -m pytest \
                  --junitxml=reports/junit.xml -v
                  '''
            }
        }
        
          stage('Quality Check')
          {
              steps {
                  sh '''
                      .venv/bin/python -m ruff check .
                      '''
              }
          } 
    }
          post {
              always {
                  junit allowEmptyResults: true,
                        testResults: 'reports/junit.xml'
              }
              success {
                  echo 'Jenkins CI Build  Succesfull'
              }
              
              failure {
                  echo 'Jenkins CI Build  Failed'
              }
              }
          
}
