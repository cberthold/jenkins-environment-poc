pipeline {
  //agent { label "docker" }    //Run everything on an agent with the docker daemon
  agent any
  parameters {
    string(name: 'INPUT_ENVIRONMENT', description: 'Environment you are building / deploying' )
    string(name: 'INPUT_BRANCH_OR_TAG', description: 'The branch or tag you are building/deploying' )
    choice(name: 'INPUT_IS_PRODUCTION', choices: ['false', 'true'], description: 'Is this a production environment?')
    booleanParam(name: 'INPUT_CONFIRMATION', description: 'The base url to override for the Identity API, leave empty to generate', defaultValue: '' )
  }
  environment {
    // convert input parameters to environment variables
    ENV_ENVIRONMENT = "${params.INPUT_ENVIRONMENT}"
    ENV_BRANCH_OR_TAG = "${params.INPUT_BRANCH_OR_TAG}"
    ENV_IS_PRODUCTION = "${params.INPUT_IS_PRODUCTION}"
    ENV_CONFIRMATION = "${params.INPUT_CONFIRMATION}"
  }
  stages {
   
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
