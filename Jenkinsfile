pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }
  stages {



    
  
          stage('Docker Bench Security') {
      steps {
        sh 'chmod +x docker-bench-security.sh'
        sh './docker-bench-security.sh'
      }
    }


     stage('SonarQube Analysis') {
          agent any
      steps {
       

       sh '/var/opt/sonar-scanner-4.7.0.2747-linux/bin/sonar-scanner -Dsonar.projectKey=shippingservice- -Dsonar.sources=. -Dsonar.host.url=http://172.31.7.193:9000 -Dsonar.token=sqp_af95d9023e0f1265a2dec5674526f02583532047'

        
      }
    }





    stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage = docker.build('koushiksai/shippingservice:latest', '.')
                }
            }
        }


    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push koushiksai/shippingservice'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
