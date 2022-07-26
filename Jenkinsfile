pipeline {
  agent any
  stages {
    stage('Build ') {
      steps {
        sh 'php -v'
        sh 'cp .env.example .env'
        sh 'composer update'
        echo 'DB_HOST=${DB_HOST} >> .env'
        echo 'DB_USERNAME=${DB_USERNAME} >> .env'
        echo 'DB_DATABASE=${DB_DATABASE} >> .env'
        echo 'DB_PASSWORD=${DB_PASSWORD} >> .env'
        sh 'php artisan key:generate'
      }
    }

    stage('Test') {
      steps {
        sh '''vendor/bin/phpunit tests/Feature
'''
      }
    }

    stage('Migrate') {
      steps {
        sh 'php artisan migrate '
      }
    }

    stage('Mail Notification') {
      steps {
        emailext(subject: 'LBC Pipeline', body: 'A mail from the above', attachLog: true, from: 'jenkins@ubuntu.com', to: 'al2a.meskine@gmail.com')
      }
    }

    stage('Create new File') {
      steps {
        prependToFile(file: 'Languages', content: 'Valar Morghulis')
      }
    }

  }
  environment {
    DB_HOST = 'localhost'
    DB_DATABASE = 'newdatabase'
    DB_USERNAME = 'credentials("laravel-username")'
    DB_PASSWORD = 'credentials("laravel-password")'
  }
}