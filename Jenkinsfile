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
        // call groovy script to execute job and save status
        script {
          def config_build = build job: 'create-env-file', parameters: [booleanParam(name: 'INPUT_CONFIRMATION', value: "${params.INPUT_CONFIRMATION}"), string(name: 'INPUT_ENVIRONMENT_TO_BUILD', value: "${params.INPUT_ENVIRONMENT}")]
          // copy the artifact out of the previous job so we can stash it for others to use later
          copyArtifacts filter: 'config.properties', fingerprintArtifacts: true, projectName: 'create-env-file', selector: specific(config_build.getId())
		  archiveArtifacts 'config.properties'
        }
        // stash the file for later use in other steps
        stash includes: 'config.properties', name: 'CONFIG_PROPERTIES'
      }
    }
    stage('Build') {
		stages {
		  stage('Stuff')
		  {
			  steps {
				  echo 'Building...'
				  unstash name: 'CONFIG_PROPERTIES'
				  sh label: 'Show configuration properties', script: 'cat config.properties'
			  }
		  }
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