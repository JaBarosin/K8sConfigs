# K8sConfigs - Bad Deployment - Kubectl

This deployment is intended to be imported, and work seamlessly, into the [CBC Container CI/CD Demo.](https://github.com/ncomeau/CBC_Container_CICD_Demo) with minimal configuration. For more information regarding the import process, please view the [K8Configs Main Branch Readme.md](https://github.com/JaBarosin/K8sConfigs/tree/main).


This deployment pipeline is expected to fail, based on intentional CBC K8s Policy Violations for resource and memory limits. 

deployment.yaml:
  - Deployment config which uses the container image built by the pipeline in **JaBarosin/NodeApp**
  - https://github.com/JaBarosin/NodeApp
