pipeline {
  agent none
  stages {
    stage('Preparation') {
      agent any
      steps {
        checkout scm
        echo 'Checking out the code from version control...'
      }
    }
    stage('Compile') {
      agent any
      steps {
        echo 'Compiling the project...'
        sh 'mvn clean compile'
      }
    }
    stage('Unit Tests') {
      agent any
      steps {
        echo 'Running unit tests...'
        sh 'mvn test'
      }
    }
    stage('Package') {
      agent any
      steps {
        echo 'Packaging the project into an artifact...'
        sh 'mvn package -DskipTests'
      }
    }
    stage('SonarQube Analysis') {
      agent any
      steps {
        withSonarQubeEnv('My SonarQube Server', credentialsId: 'a9d915e5-5dbc-4e7d-8ad0-d05c1f4f22ee') {
          echo 'Running SonarQube analysis...'
          sh 'mvn sonar:sonar'
        }
      }
    }
  }
}
