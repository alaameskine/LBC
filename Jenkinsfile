pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'php -v'
        sh 'cp .env.example .env'
        sh 'composer update'
        sh 'php artisan key:generate'
        sh 'sed -i -e \'s/DB_DATABASE=homestead/DB_DATABASE=staging/g\' .env'
        sh 'sed -i -e \'s/DB_USERNAME=homestead/DB_USERNAME=yourusername/g\' .env'
        sh 'sed -i -e \'s/DB_PASSWORD=secret/DB_PASSWORD=yourpassword/g\' .env'
      }
    }

    stage('Test') {
      steps {
        sh 'vendor/bin/phpunit tests/Feature'
      }
    }

    stage('Migrate') {
      steps {
        sh 'php artisan migrate'
      }
    }

    stage('Mail Notification') {
      steps {
        emailext(subject: 'LBC Pipeline', body: 'Build Successful', to: 'al2a.meskine@gmail.com', from: 'jenkins@ubuntu')
      }
    }

    stage('Create new file') {
      steps {
        prependToFile(file: 'Languages', content: 'A new file is born today')
      }
    }

  }
  environment {
    DB_HOST = 'localhost'
    DB_DATABASE = 'credentials("laravel_database")'
    DB_USERNAME = 'credentials("laravel_username")'
    DB_PASSWORD = 'credentials("laravel_password")'
  }
}