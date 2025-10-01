pipeline {
  agent { label 'Jenkins-Agent' }
  
  tools {
    jdk 'Java_17'
    maven 'Maven_3'
  }
  
  stages {
    stage("Cleanup Workspace") {
      steps {
        cleanWs()  // lowercase 's' is correct method name
      }
    }
    
    stage("Checkout from SCM") {
      steps {
        git branch: 'main', credentialsId: 'github', url: 'https://github.com/PrachiSukreSS/DevOps_Project'
      }
    }
    
    stage("Build Application") {
      steps {
        sh "mvn clean package"
      }
    }
    
    stage("Test Application") {
      steps {
        sh "mvn test"
      }
    }
  }
}
