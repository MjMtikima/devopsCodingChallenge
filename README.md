# devopsCodingChallenge

Architecture:
![image](https://github.com/MjMtikima/devopsCodingChallenge/assets/116918088/2b9477ae-489e-4c8b-b3c3-53f4077e0d45)

Step1: Setup of a Basic Kubernetes cluster (master) with 2 nodes. The configuration files are
-
Result in Screenshot
![kubernetes_master_status](https://github.com/MjMtikima/devopsCodingChallenge/assets/116918088/8f8ef71a-0f9e-44fd-8df1-50a6dba616af)
![kubernates_kuard_pod](https://github.com/MjMtikima/devopsCodingChallenge/assets/116918088/ab94afb2-387f-415b-b4bf-d8aed697c1cd)


Step2: Deployment of Demo application which can be found under https://github.com/kubernetes-up-and-running/kuard
- service.yaml
- deployment.yaml
Step3: We setup a 2 nodes of the application


All applications are offered with a template creation tool of your choice (helm, ytt, kustomize, ...)
chest provided
