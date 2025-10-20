pipeline {
  agent {
    docker { image 'node:20' }
  }
  options { ansiColor('xterm') }
  environment { CI = 'true' }
  stages {
    stage('Checkout') { steps { checkout scm } }
    stage('Install')   { steps { sh 'npm ci' } }
    stage('Lint & Test') {
      steps {
        sh 'npm run lint --if-present'
        sh 'npm test --if-present'
      }
    }
    stage('Build')     { steps { sh 'npm run build' } }
    stage('Archive (export)') {
      when { expression { fileExists('out') } }
      steps { archiveArtifacts artifacts: 'out/**', onlyIfSuccessful: true }
    }
    stage('Archive (standalone)') {
      when { expression { fileExists('.next/standalone') } }
      steps {
        archiveArtifacts artifacts: '.next/standalone/**, .next/static/**, public/**, package.json', onlyIfSuccessful: true
      }
    }
  }
}