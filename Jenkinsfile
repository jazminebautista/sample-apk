pipeline {
  agent any

  stages {

/*
    stage('Build') {
      steps {
        sh 'npm install'
      }
    }
*/

    stage('ZeroNorth Integration Artifact Uploader') {
      steps {
        script {
          /* Retrieve the API token from the Credentials Store */
          withCredentials([[$class: 'StringBinding', credentialsId: 'zn_api_key', variable: 'ZN_API_KEY']]) {
            znApiKey = "${env.ZN_API_KEY}".toString()
          }

          /* Get ZeroNorth Docker images */
          def image = docker.image("cybricio/integration:latest")
          image.pull()

          /* Scan #1: SonarQube scan */
          image.inside("-v \"${WORKSPACE}/:/results\" -v \"$WORKSPACE:/code\" -e CYBRIC_API_KEY=${znApiKey} -e POLICY_ID=bTUkgz6RQNCfi9h_ZkzY8g") {
            sh "python /app/cybric.py"
          }

        }
      }
    }
  }
}





