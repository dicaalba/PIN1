pipeline {
  agent any

  options {
    timeout(time: 2, unit: 'MINUTES')
  }

  environment {
    ARTIFACT_ID = "elbuo8/webapp:${env.BUILD_NUMBER}"
  }
   stages {
   stage('Building image') {
      steps{
          sh '''
          cd webapp
          docker build -t testapp .
             '''  
        }
    }
  
  
    stage('Run tests') {
      steps {
        sh "docker run testapp npm test"
      }
    }
   stage('Deploy Image') {
      steps{
        sh '''
        docker tag testapp 127.0.0.1:5000/mguazzardo/testapp
        docker push 127.0.0.1:5000/mguazzardo/testapp   
        '''
        }
      }
   stage('Vulnerability Scan - Docker ') {
        steps {
          sh "docker run  -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image --severity=critical 127.0.0.1:5000/mguazzardo/testapp"
            }
        }
    }
}
