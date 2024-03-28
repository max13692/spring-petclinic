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
        archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
      }
    }
    stage('SonarQube Analysis') {
      agent any
      steps {
        withSonarQubeEnv(installationName: 'My SonarQube Server') {
          echo 'Running SonarQube analysis...'
          sh 'mvn clean package' 
          sh 'mvn sonar:sonar'
        }
      }
    }
    stage('Deploy') {
    agent any
    steps {
        echo 'Deploying the application within the current container...'
        sh 'nohup java -jar *.jar --server.port=9090'
    }
    }
  }
}
