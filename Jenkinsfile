node {

  violations = false
  
  // Cloning the Repository to jenkins-docker Workspace
  stage('Get Deployment Files') {
      
      checkout scm
  }
  
  // Runs cbctl to validate yaml against policy for violations - dump output to json file
  stage('Validate K8s-object') {
  try {
    echo "Validate stage... Starting validate test for deployment-v2.yaml."
    sh '/var/jenkins_home/app/cbctl k8s-object validate -f deployment-v2.yaml -o json > deployment_manifest_validate_v2.json'

  }

  catch (err) {
    violations = true
    echo "Build detected cbctl violations. Review Cbctl scan results."
    
    }

    // Post the results to Slack, upon success or failure, of cbctl validation against policy rules
    stage('Slack Post - Validate k8s-object') {
        
        // Simple loop added to allow python slack helper scripts, built within the ova, to work
         try{
            sh 'git clone https://github.com/slackapi/python-slack-sdk.git'
         }
         catch(exists){
             echo 'already exists'
         }
          // If no CBC K8s policy violations; confirm successful build via Slack - move to deployment of validated k8
          if(violations == false) {
            echo "No violations occured - keen!"
            sh "python3 /var/jenkins_home/app/success.py '${env.JOB_NAME}' '${env.BUILD_NUMBER}'"            
          }
          
          // If CBC K8s policy violations triggered; output those violations to Slack, end deployment pipeline to allow for rectifying of violations prior to prod deployment
          if(violations == true) {
            sh 'python3 /var/jenkins_home/app/k8s_validate_slack.py deployment_manifest_validate_v2.json'

            sh "python3 /var/jenkins_home/app/failure.py '${env.JOB_NAME}' '${env.BUILD_NUMBER}' '${env.STAGE_NAME}'"
            
            echo "Violations occured. results of cbctl validate can be found in deployment_manifest_validate.json"
            error("Failed Deployment due to CB Container policy violations.")
          }
      }
   }
}

// Create kubectl Node & Container template for utilization in deployment process
def k8 = "jenkins-k8-${UUID.randomUUID().toString()}"
podTemplate(label: k8, containers: [
containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:v1.8.8', command: 'cat', ttyEnabled: true)
])
{
node (k8) {
        // Get Validated Deployment files with no Violations, based on tests run above
        stage ('Get Validated Deployment Files') {
            container('kubectl') {
              checkout scm
            }   
        }
  
      // Create nodeapp namespace, and deploy validated yaml
      stage ('Deploy Final Image') {
            container('kubectl') {
              try{
                    sh "kubectl create namespace nodeapp"
              }
              catch(exists2){
                echo "namespace already exists...moving on"
              }
                    sh "kubectl apply -f deployment-v2.yaml -n nodeapp --validate=false"
            }
        }
  
  // Remove prior deployed nodeapp container, for hygiene purposes
  // Navigate to localhost:30333 on your demo vm to see your static site!
  stage ('Cleaning Up Image - After 1 min for Viewing'){
    container('kubectl') {
      echo "Navigate to localhost:30333 on your demo vm to see your static site!"
      sleep 60
      sh "kubectl get all -n nodeapp"
      sh "kubectl delete service --all -n nodeapp"
      sh "kubectl delete deployment --all -n nodeapp"

    }
  }
}
}

