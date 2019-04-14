pipeline{
	agent any
	parameters {
		text(name: 'master_config', defaultValue: '', description: 'Optional section that holds parameters common for every pipeline. Populate it if the default parameters are not acceptable')
		text(name: 'pipeline_config', defaultValue: '', description: 'Optional section that holds parameters specific to pipeline. Populate it if the default parameters are not acceptable')
	}
	
	stages{
	
		stage("Preparation") {
				// Every stage must have a steps block containing at least one step.
                steps {
                    // Get some code from a GitHub repository
                    println "I am in the stage - Preparation stage ..... "
                    git 'https://github.com/duggidk/AWS-CodePipeline.git'
                }
		}
		stage("Plan") {
				// Every stage must have a steps block containing at least one step.
                steps {
                    // Get some code from a GitHub repository
                    println "I am in the stage - Plan stage.... "
                    git 'https://github.com/duggidk/AWS-CodePipeline.git'
                    
                    script{
                    	sh 'terraform init'
                    }
                }
		}
		
		          
		
		stage ('Input') {
			steps {
				script {
					// Call job populateInput to populate genesis.tfvars file & placeholders
					println"I am in Input stage. "
					build job: '/populateInput', parameters:
						//[[$class: 'StringParameterValue', name: 'workspace_dir', value: env['WORKSPACE']],
						 //[$class: 'StringParameterValue', name: 'master_config', value: params ['master_config']]
						 [$class: 'StringParameterValue', name: 'pipeline_config', value: params['pipeline_config']]
						// [$class: 'StringParameterValue', name: 'pipeline_default_config', value: 'default_nexus_configuration']]
						 wait:true
				}
			}
		}
		
		
	}										
								 
    
   } 						 		