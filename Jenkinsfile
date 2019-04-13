pipeline{
	agent any
	parameters {
		text(name: 'master_config', defaultValue: '', description: 'Optional section that holds parameters common for every pipeline. Populate it if the default parameters are not acceptable')
		text(name: 'pipeline_config', defaultValue: '', description: 'Optional section that holds parameters specific to pipeline. Populate it if the default parameters are not acceptable')
	}
	
	stages{
		stage ('Input') {
			steps {
				script {
					// Call job populateInput to populate genesis.tfvars file & placeholders
					build job: '/populateInput', parameters:
						[[$class: 'StringParameterValue', name: 'workspace_dir', value: env['WORKSPACE']],
						 [$class: 'StringParameterValue', name: 'master_config', value: params['master_config']]
						 [$class: 'StringParameterValue', name: 'pipeline_config', value: params['pipeline_config']]
						 [$class: 'StringParameterValue', name: 'pipeline_default_config', value: 'default_nexus_configuration']]
						 wait:true
				}
			}
		}
		
		stage ('Nexus_Chef') {
			when { expression { params.upstreamJob != 'destroy' }}
			steps {
				build 'Nexus-Chef/feature%2Fgenesis'
			}
		}
		
		stage ('Plan') {
			when { expression {params.upstreamJob != 'destroy' }}
			steps {
				script{
					sh 'terraform init'
					sh 'terraform plan -var-file=genesis.tfvars'
				}
			}
		}
		
		stage ('Output') {
			when { expression { params.upstreamJob != 'destroy'}}
			steps {
				script {
					sh 'terraform output > nexus_output.tfvars'
				}
			}
		}
		
		stage ('Destroy') {
			when { expression {params.upstreamJob == 'destroy' }}
			steps {
				script {
					sh 'terraform destroy -var-file=genesis.tfvars -input=false -auto-approve'
				}
			}
		}
		post {
			always {
				archiveArtifacts(artifacts: "nexus_output.tfvars", fingerrprint: false)
			}
		}													
								 
						 		