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
    stage('Build docker img') {
      steps {
        sh 'docker build -t fronttest . '
      }
    }
    stage('Deploy') {
      steps {
          sh 'docker run -dit --name fronttest --restart unless-stopped -p 3030:80 fronttest'
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