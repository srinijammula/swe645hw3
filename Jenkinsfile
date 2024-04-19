pipeline{
	agent any
	environment {
		DOCKERHUB_PASS = "University@123"
	}
	stages{
		stage("Generating the Build for SWE645 student survey form"){
			steps{
				script{
					checkout scm
					sh 'rm -rf *.jar'
					sh 'mvn clean install'
					sh "echo ${DOCKERHUB_PASS} | docker login -u srinijammula --password-stdin"
					sh 'docker build -t srinijammula/hw3demo1 .'
				}
			}
		}
		stage("Pushing image to docker"){
			steps{
				script{
					sh 'docker push srinijammula/hw3demo1'
				}
			}
		}
		stage("Deploying to rancher"){
			steps{
				script{
				
					String kubeconfigPath = '/var/lib/jenkins/.kube/config'
            
					sh "kubectl --kubeconfig=${kubeconfigPath} apply -f deployment.yaml"
					sh "kubectl --kubeconfig=${kubeconfigPath} apply -f service.yaml"

					// Force a new rollout for the deployment
            		sh "kubectl --kubeconfig=/var/lib/jenkins/.kube/config rollout restart deployment/deployment"
            
					// Assuming your deployment's name is 'deployment', replace 'deployment' with your actual deployment name if different
					sh "kubectl --kubeconfig=${kubeconfigPath} rollout status deployment/deployment"
				}
			}
		}
	}
}