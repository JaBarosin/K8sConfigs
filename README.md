# K8sConfigs - Good Deployment - Kubectl

This deployment is intended to be imported, and work seamlessly, into the [CBC Container CI/CD Demo](https://github.com/ncomeau/CBC_Container_CICD_Demo) with minimal configuration. For more information regarding the process for importing this pipeline, please view the [K8Configs Main Branch Readme.md](https://github.com/JaBarosin/K8sConfigs/tree/main).

This deployment pipeline section is intended to highlight a successful deployment pipeline, after changes to the the yaml have been made, to align with the CBC Container Security Policy rules.

deployment-v2.yaml:
  - Deployment config which uses the container image built by the pipeline in **JaBarosin/NodeApp**
  - https://github.com/JaBarosin/NodeApp
