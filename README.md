# CB Container Demo - K8s configs, Jenkinsfiles, and more!

This repo has several example branches which can be used by SEs to test different K8s resource configurations in a demo environment.

In order to get started, start by setting up your environment by following the [CBC_Container_CICD_Demo_Env](https://github.com/ncomeau/CBC_Container_CICD_Demo_Env) guide.

## Branches

#### _CBC_Container_Demo_

  * Validate and deploy two different k8s configurations to a microk8s cluster, in an effort to highlight how cbctl can be utilized within a deployment pipeline. The intent of this demo is to first highlight how you can practice good hygiene, by identifying potential vulnerabilities and CBC Policy Violations in the build cycle, enabling a proactive posture and optimization of a CI/CD deployment pipeline. 
    * Start with the 'bad' deployment - this deployment contains configurations that violate the demo CB Container Policy rules, and terminates the pipeline prior to the deployment phase.
    * Finish with the 'good' deployment - this deployment successfully completes, devoid of any CBC Container Policy violations, and allows for completion of the deployment pipeline, generating a workload of a static site, which is monitored by CB Container resources.

###### Setup for Jenkins

  1. Navigate to your Jenkins Dashboard

  2. Select ```New Item``` on the upper left-hand side

![image](https://user-images.githubusercontent.com/18126247/127362958-2ca2258f-6f6e-4552-ad4c-0092acbff7cf.png)


  3. Enter a name for your project (i.e. "Bad-Deployment-Pipeline")--> select ```Pipeline``` --> click 'ok' in the bottom left

![image](https://user-images.githubusercontent.com/18126247/127363645-2d59e660-cb30-4401-b9d9-9e29cf536520.png)

  4. Check the box entitled ```Github Project```
     * Import in the repo url - in this case it would simply be: ```https://github.com/JaBarosin/K8sConfigs```
     ![image](https://user-images.githubusercontent.com/18126247/127365401-7147c4a2-28b2-4b15-8d73-d78391e3b998.png)


  5. Inside of the pipeline configuration page enter the following
     1. Change **Definition** field to ```Pipeline script from SCM```
     2. Change **SCM** field to ```Git```
     3. Change **Repository URL** to ```https://github.com/JaBarosin/K8sConfigs.git```
     4. Change **Branch Specifier** to ```*/CBC_Container_Demo```
        * _Ignore the red ✖️, that always happens_
     5. Change **Script Path** to ```bad/Jenkinsfile```
     6. Lastly, click ```Apply``` & ```Save```
     ![image](https://user-images.githubusercontent.com/18126247/127366311-de60fa9c-8c0f-46a5-85c8-100d24cb0500.png)

Pipeline config | Value
--------------------- | ---------------------
Pipeline Definition: | Pipeline script from SCM
SCM: | Git
Repositories - Reposiroty URL: | ```https://github./com/JaBarosin/K8sConfigs.git```
Credentials: | _none_
Branches to build - Branch specifier: | ```*/CBC_Container_Demo```
Repository Browser: | (Auto)
Script Path: | ```bad/Jenkinsfile```


  6. Repeat steps 1-3 and create a second Pipeline.
     * Create the second Pipeline with a name like "Good-Deployment-Pipeline"
     * Set the _Script Path_ to ```good/Jenkinsfile```

  7. Select ```Build Now``` for the Job set to the _bad_ job first.
     * View the slack notification which confirms the violations, and provides a breakdown into what specific configurations were the offending rules.

  8. Select ```Build Now``` for the Job set to the _good_ job.

  * View the slack notifications.
  * Curl or navigate to <your-IP-address>:30333 and check to see the container webpage.
  * _Tip_: On your Jenkins host, open a new terminal tab and use the ```watch``` command to monitor microk8s.
    - Enter command: ```watch microk8s kubectl get all -n nodeapp``` (crtl+c or cmd+c = exit)
