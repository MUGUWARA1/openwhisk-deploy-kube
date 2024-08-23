# OpenWhisk Deployment on Minikube

This guide will walk you through deploying OpenWhisk on Minikube using Helm, configuring it for local development, and testing a simple action.

## Prerequisites

- Minikube installed and running
- Helm installed and configured
- wsk CLI installed


### Step 1: Clone the OpenWhisk Deployment Repository

Clone the official OpenWhisk Kubernetes deployment repository:

```   
 git clone https://github.com/apache/openwhisk-deploy-kube.git 
 cd openwhisk-deploy-kube
```

### Step 2: Add OpenWhisk Helm Repository

Add the OpenWhisk Helm repository and update it:

```
helm repo add openwhisk https://openwhisk.apache.org/charts
helm repo update
```

### Step 3: Deploy OpenWhisk with Helm

Deploy OpenWhisk using Helm with a custom configuration file:
```
helm install owdev openwhisk/openwhisk -n openwhisk --create-namespace -f /deploy/docker-windows/mycluster.yaml
```
### Step 4: Configure wsk CLI

Set the API host for wsk:
```
wsk property set --apihost http://127.0.0.1:63098
```

Forward the controller service port to your local machine:
```
kubectl port-forward -n openwhisk svc/owdev-controller 63098:8080
```
Set the authentication key for wsk:
```
wsk property set --auth-key XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXx
```

Replace XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX with your actual authentication key.

### Step 5: Label Minikube Nodes for OpenWhisk Invokers
Label the Minikube node so that OpenWhisk can identify which node should run the invoker component:
```
kubectl label nodes --all openwhisk-role=invoker
```
### Step 6: Test a Simple OpenWhisk Action

**Create an Action**
```
wsk action create name_of_action FILE_of_action
```
Create a simple JavaScript file, ac1.js, with the following content:
```       
    function main(){

        return {
            result : "Hello World"
        };
    }
    
```
**Example :**
Use the wsk CLI to create an action from this file:
```
wsk action create ac1 ac1.js
```

**Invoke the Action**
Invoke the action to see the result:
```
wsk action invoke ac1 --result 
```
Expected output:

```
{
    "result": "Hello World"
}

```


# Setting Up the wsk CLI for OpenWhisk

The wsk CLI (OpenWhisk Command Line Interface) is a tool that allows you to interact with your OpenWhisk deployment.

## Download and Install the wsk CLI
Download the wsk CLI:

**Go to the Apache OpenWhisk CLI Releases page.**
https://github.com/apache/openwhisk-cli/releases

Download the appropriate version of the wsk CLI for your operating system (e.g., macOS, Linux, Windows).
