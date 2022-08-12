pipeline {
  agent any
  stages {
    stage("verify tooling") {
      steps {
        sh '''
          docker version
        '''
      }
    }
    stage('Prune Docker data') {
      steps {
        sh 'docker system prune -a --volumes -f'
      }
    }
    stage('Start container and build') {
      steps {
        sh 'docker compose up --build -d --no-color --wait'
        sh 'docker compose ps'
      }
    }
    stage('Run tests against the container') {
      steps {
        sh 'curl http://localhost:3000 | jq'
      }
    }
  }
  post {
    always {
      sh 'docker compose down --remove-orphans -v'
      sh 'docker compose ps'
    }
  }
}