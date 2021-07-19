node {

  violations = false

  stage('Get deployment files') {
      /* Cloning the Repository to jenkins-docker Workspace */
      // git branch: 'jake-testing', url: 'https://github.com/JaBarosin/NodeApp.git'
      checkout scm
  }

  stage('Validate K8s-object') {
  try {
    echo "Validate stage... Starting validate test for deployment-v2.yaml."
    sh '/var/jenkins_home/app/cbctl k8s-object validate -f deployment-v2.yaml -o json > deployment_manifest_validate_v2.json'
    // sh 'python3 /var/jenkins_home/app/cbctl_validate_helper.py ${REPO}_${IMAGE}_validate.json > cbctl_policy_no_violations.txt'

  }

  catch (err) {
    violations = true
    echo "Build detected cbctl violations. Review Cbctl scan results."
    // sh 'python3 /var/jenkins_home/app/cbctl_validate_helper.py ${REPO}_${IMAGE}_validate.json > cbctl_policy_violations.txt'

    }

    stage('Slack Post - Validate k8s-object') {

         try{
            sh 'git clone https://github.com/slackapi/python-slack-sdk.git'
         }
         catch(exists){
             echo 'already exists'
         }

          if(violations == false) {
            echo "No violations occured - keen!"
            sh "python3 /var/jenkins_home/app/success.py '${env.JOB_NAME}' '${env.BUILD_NUMBER}'"
            //sh "python3 /var/jenkins_home/app/final_stage_success.py '${env.JOB_NAME}' '${env.BUILD_NUMBER}'"

            
          }

          if(violations == true) {
            sh 'python3 /var/jenkins_home/app/k8s_validate_slack.py deployment_manifest_validate_v2.json'

            sh "python3 /var/jenkins_home/app/failure.py '${env.JOB_NAME}' '${env.BUILD_NUMBER}' '${env.STAGE_NAME}'"
            
            echo "Violations occured. results of cbctl validate can be found in deployment_manifest_validate.json"
            error("Failed Deployment due to CB Container policy violations.")
          }
      }
   }
}


def k8 = "jenkins-k8-${UUID.randomUUID().toString()}"
podTemplate(label: k8, containers: [
containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:v1.8.8', command: 'cat', ttyEnabled: true)
])
{
node (k8) {
        stage ('Get Validated Deployment Files') {
            container('kubectl') {
              checkout scm
            }   
        }

      stage ('Deploy Final Image') {
            container('kubectl') {
                    sh "kubectl apply -f deployment-v2.yaml -n feline --validate=false"
                    sh "kubectl get all -n feline"
            }
        }
  stage ('Cleaning Up Image - After 1 min for Viewing'){
    container('kubectl') {
      sleep 60
      sh "kubectl get all -n feline"
      sh "kubectl delete deployment --all -n feline"
      sh "kubectl delete service --all -n feline"
    }
  }
}
}



/*
  stage('Deployment test') {
    sshagent(['ubuntu-host']) {
      sh "scp -o StrictHostKeyChecking=no deployment-v2.yaml jake@192.168.6.44:/k8s/dev/"
      try{
          sh "ssh jake@192.168.6.44 microk8s kubectl apply -f /k8s/dev/deployment-v2.yaml && sleep 5"
      }

      catch(error){
          echo "Welp... those didnt exist yet..."
          sh "ssh jake@192.168.6.44 microk8s kubectl create -f /k8s/dev/deployment-v2.yaml && sleep 5"
      }

      stage('Connection Test') {
        sh "curl 192.168.6.44:30333"
        echo "Done testing"
      }

      stage('Cleanup') {
        sh "ssh jake@192.168.6.44 microk8s kubectl delete deployment nodeapp"
        sh "ssh jake@192.168.6.44 microk8s kubectl delete service nodeapp-service"
        sh "ssh jake@192.168.6.44 microk8s kubectl get all"

      }

    }

  }

}
*/
