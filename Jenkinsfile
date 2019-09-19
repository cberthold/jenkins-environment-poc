pipeline {
  //agent { label "docker" }    //Run everything on an agent with the docker daemon
  agent any
  parameters {
    string(name: 'INPUT_ENVIRONMENT', description: 'Environment you are building / deploying' )
    string(name: 'INPUT_BRANCH_OR_TAG', description: 'The branch or tag you are building/deploying' )
    choice(name: 'INPUT_IS_PRODUCTION', choices: ['false', 'true'], description: 'Is this a production environment?')
    booleanParam(name: 'INPUT_CONFIRMATION', description: 'The base url to override for the Identity API, leave empty to generate', defaultValue: '' )
  }
  stages {
   
    stage('Configuration') {
      steps {
        echo 'Configuring...'
        build job: 'create-env-file',
		parameters: [
			string(name: 'INPUT_ENVIRONMENT_TO_BUILD', value: "${params.INPUT_ENVIRONMENT}"),
			booleanParam(name: 'INPUT_CONFIRMATION', value: "${params.INPUT_CONFIRMATION}"),
		]
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
