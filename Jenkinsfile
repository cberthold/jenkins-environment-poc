pipeline {
  agent { label "docker" }    //Run everything on an agent with the docker daemon
  environment {
    IMAGE = readMavenPom().getArtifactId()    //Use Pipeline Utility Steps
    VERSION = readMavenPom().getVersion()
  }
  stages {
    stage('Input') {
      steps {
        script {
          input {
            message 'Confirmation'
            id 'DEPLOYMENT_INPUT'
            ok 'Build/Deploy'
            submitterParameter 'CONFIRMED_BY'
            parameters {
              booleanParam defaultValue: false, description: 'Confirmation that we want to build or deploy the environment', name: 'INPUT_CONFIRMATION'
              string defaultValue: 'qa2', description: 'the environment that will be built/deployed', name: 'INPUT_ENVIRONMENT', trim: false
              string defaultValue: 'develop', description: 'The branch or tag that will be built/deployed', name: 'INPUT_BRANCH_TAG', trim: false
              booleanParam defaultValue: true, description: 'For environments that allow building, this selects if we should make another build or try re-deploying the last good branch/tag', name: 'INPUT_SHOULD_BUILD'
            }
          }
        }
      }
    }
    stage('Configuration') {
      steps {
        echo 'Configuring...'
        build job: 'create-env-file', parameters: [booleanParam(name: 'INPUT_CONFIRMATION', value: true), string(name: 'INPUT_ENVIRONMENT_TO_BUILD', value: 'qa')]
      }
    }
    stage('Build') {
      steps {
          echo 'Building...'
      }
    }
    stage('Deploy') {
      steps {
        echo 'Deploying...'
      }
    }
    stage('Cleanup') {
      steps {
        echo 'Cleaning Up...'
      }
    }
  }
}
