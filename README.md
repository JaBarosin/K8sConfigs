# CBC Container Demo artifacts

This demos two Jenkins-K8s deployments. It is intended to be imported, and work seamlessly, into the [CBC Container CI/CD Demo](https://github.com/ncomeau/CBC_Container_CICD_Demo) with minimal configuration. For more information regarding the process for importing this pipeline, please view the [K8Configs Main Branch Readme.md](https://github.com/JaBarosin/K8sConfigs/tree/main).


## Bad-deployment
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

### _Jenkins_
![image](https://user-images.githubusercontent.com/18126247/127372673-ef2489a0-c480-49a4-8f1b-ed4159d186a1.png)

### _Slack_
![image](https://user-images.githubusercontent.com/18126247/127372960-86a9fbb1-c389-4e9f-817c-bce33e8e12f0.png)


## Good-deployment
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

### _Jenkins_
![image](https://user-images.githubusercontent.com/18126247/127372856-f8ad8871-5965-44a7-8a0a-832db07e4ba3.png)

### _Slack_
![image](https://user-images.githubusercontent.com/18126247/127373075-eda903c4-7c33-4e79-b0be-83cda11a525b.png)

