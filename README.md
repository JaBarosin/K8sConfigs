# CBC Container Demo Breakdown

This repo is intended to be imported, and work seamlessly, into the [CBC Container CI/CD Demo](https://github.com/ncomeau/CBC_Container_CICD_Demo) with minimal configuration. Please see below for proper configuration and setup of the demo itself.

Validate and deploy two different k8s configurations to a microk8s cluster, in an effort to highlight how cbctl can be utilized within a deployment pipeline. The intent of this demo is to first highlight how you can practice good hygiene, by identifying potential vulnerabilities and CBC Policy Violations in the build cycle, enabling a proactive posture and optimization of a CI/CD deployment pipeline. 
  * Start with the ***'bad'*** deployment - this deployment contains configurations that violate the demo CB Container Policy rules, and terminates the pipeline prior to the deployment phase.
  * Finish with the ***'good'*** deployment - this deployment successfully completes, devoid of any CBC Container Policy violations, and allows for completion of the deployment pipeline, generating a workload of a static site, which is monitored by CB Container resources.

## Index
 * [Jenkins Setup](https://github.com/JaBarosin/K8sConfigs/tree/CBC_Container_Demo#jenkins-setup)
 * [CBC Setup](https://github.com/JaBarosin/K8sConfigs/tree/CBC_Container_Demo#cbc-setup)
 * [Run/Build Options](https://github.com/JaBarosin/K8sConfigs/tree/CBC_Container_Demo#jenkins-setup)

## Jenkins Setup

  1. Navigate to your Jenkins Dashboard

  2. Select ```New Item``` on the upper left-hand side

![image](https://user-images.githubusercontent.com/18126247/127362958-2ca2258f-6f6e-4552-ad4c-0092acbff7cf.png)


  3. Enter a name for your project (i.e. ***bad-deployment-pipeline***)--> select ```Pipeline``` --> click 'ok' in the bottom left

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


  6. Repeat steps 1-3 and create a second Pipeline.
     * Create the second Pipeline with a name of; ***good-deployment-pipeline***
     * Set the _Script Path_ to ```good/Jenkinsfile```

## CBC Setup

***content pending***
   
## Run/Build Options

### _Option 1: Run Builds Individually_

  1. Select ```Build Now``` for the Job set to the ***bad-deployment-pipeline*** job first.
     * View the slack notification which confirms the violations, and provides a breakdown into what specific configurations were the offending rules.

  2. Select ```Build Now``` for the Job set to the ***good-deployment-pipeline*** job.
     * View the slack notification confirming success.
     * Curl or navigate to \<your-demo-vm-IP-address\>:30333 and check to see the container webpage.
       * _Tip_: On your Jenkins host, open a new terminal tab and use the ```watch``` command to monitor microk8s.
          - Enter command: ```watch microk8s kubectl get all -n nodeapp``` (crtl+c or cmd+c = exit)

### _Option 2: Link Both Builds Together_

  1. Select ```Configure``` for the ***good-deployment-pipeline***.
  ![image](https://user-images.githubusercontent.com/18126247/127369652-a0d75a7d-9a24-47cd-9cdb-1f49370c9f05.png)
  
  2. In the ```Build Triggers``` Section:
     1.  Check Box next to ```Build after other projects are built```
     2.  Under ```Projects to watch``` enter your bad deployment's name (i.e. ***bad-deployment-pipeline***)
     3.  Select the radio button for ```Trigger even if build fails```
     ![image](https://user-images.githubusercontent.com/18126247/127370396-81d014a9-b1d9-431f-84c4-d9b2132bcfa3.png)
     
  3. Navigate back to your ***bad-deployment-pipeline*** & Select ```Build Now```
     * This will first run your ***bad-deployment-pipeline*** - which will fail, as expected
       *  View the slack notification which confirms the violations, and provides a breakdown into what specific configurations were the offending rules.
     * However, upon failure, it will automatically trigger ```Build Now``` for your ***good-deployment-pipeline*** - which will succeed, as expected
       * View the slack notification confirming success.
       * Curl or navigate to \<your-demo-vm-IP-address\>:30333 and check to see the container webpage.
         * _Tip_: On your Jenkins host, open a new terminal tab and use the ```watch``` command to monitor microk8s.
            - Enter command: ```watch microk8s kubectl get all -n nodeapp``` (crtl+c or cmd+c = exit)



## Specific Configs & Output

### Bad-deployment
  - Jenkinsfile and deployment.yaml that violate CB Container policy rules
  - Pipeline Config for ***bad-deployment-pipeline***:
  
  Pipeline config | Value
--------------------- | ---------------------
Pipeline Definition: | Pipeline script from SCM
SCM: | Git
Repositories - Reposiroty URL: | ```https://github./com/JaBarosin/K8sConfigs.git```
Credentials: | _none_
Branches to build - Branch specifier: | ```*/CBC_Container_Demo```
Repository Browser: | (Auto)
Script Path: | ```bad/Jenkinsfile```

#### _Jenkins_
![image](https://user-images.githubusercontent.com/18126247/127372673-ef2489a0-c480-49a4-8f1b-ed4159d186a1.png)

#### _Slack_
![image](https://user-images.githubusercontent.com/18126247/127372960-86a9fbb1-c389-4e9f-817c-bce33e8e12f0.png)


### Good-deployment
  - Jenkinsfile and deployment-v2.yaml updated with additional configurations to satisfy CB Container policy rules
  - Pipeline Config for ***good-deployment-pipeline***:

Pipeline config | Value
--------------------- | ---------------------
Pipeline Definition: | Pipeline script from SCM
SCM: | Git
Repositories - Reposiroty URL: | ```https://github./com/JaBarosin/K8sConfigs.git```
Credentials: | _none_
Branches to build - Branch specifier: | ```*/CBC_Container_Demo```
Repository Browser: | (Auto)
Script Path: | ```good/Jenkinsfile```

#### _Jenkins_
![image](https://user-images.githubusercontent.com/18126247/127372856-f8ad8871-5965-44a7-8a0a-832db07e4ba3.png)

#### _Slack_
![image](https://user-images.githubusercontent.com/18126247/127373075-eda903c4-7c33-4e79-b0be-83cda11a525b.png)

