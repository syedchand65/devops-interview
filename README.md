# devops-interview

Here are answers to your Azure DevOps and Kubernetes interview questions:  

---

### **Azure DevOps: Pipelines & Project Structure**  
#### **1. Pipeline and Project Structure**  
A **typical Azure DevOps pipeline** consists of:  
- **Stages** (e.g., Build, Test, Deploy)  
- **Jobs** (Parallel execution units within a stage)  
- **Tasks** (Commands or scripts to execute)  
- **Artifacts** (Files produced and passed between stages)  

Example **YAML Pipeline Structure**:  
```yaml
trigger:
  - main

stages:
- stage: Build
  jobs:
  - job: BuildJob
    steps:
    - task: DotNetCoreCLI@2
      inputs:
        command: 'build'
        projects: '**/*.csproj'

- stage: Deploy
  jobs:
  - job: DeployJob
    steps:
    - task: AzureWebApp@1
      inputs:
        appName: 'my-app'
        package: '$(Build.ArtifactStagingDirectory)/my-app.zip'
```  
---
### **Kubernetes**  
#### **2. Kubernetes ReplicaSet**  
A **ReplicaSet** ensures a specified number of **pod replicas** are running at any time.  
Example YAML:  
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-replicaset
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx
```  
---
#### **3. Kubernetes HPA & VPA**  
- **HPA (Horizontal Pod Autoscaler)** → Scales out/in pods based on CPU, Memory.  
  ```yaml
  apiVersion: autoscaling/v2
  kind: HorizontalPodAutoscaler
  metadata:
    name: my-hpa
  spec:
    scaleTargetRef:
      apiVersion: apps/v1
      kind: Deployment
      name: my-app
    minReplicas: 2
    maxReplicas: 10
    metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50
  ```  
- **VPA (Vertical Pod Autoscaler)** → Adjusts pod CPU/memory requests.  
---
### **Docker Optimization**  
#### **4. How to Reduce Docker Image Size?**  
- Use **Alpine-based images** (e.g., `node:alpine`).  
- Remove **unnecessary dependencies**.  
- Use **multi-stage builds**.  
- Clean up unused files using `rm -rf`.  
Example:  
```dockerfile
FROM node:alpine AS builder
WORKDIR /app
COPY package.json . 
RUN npm install 

FROM node:alpine
WORKDIR /app
COPY --from=builder /app .
CMD ["node", "index.js"]
```  
---
### **Azure Security**  
#### **5. Azure Secrets & Key Vault**  
- **Azure Key Vault** stores sensitive data (e.g., passwords, connection strings).  
- Secrets can be accessed via **Azure DevOps Pipeline**, Kubernetes, or applications.  
Example to fetch a secret in a pipeline:  
```yaml
- task: AzureKeyVault@1
  inputs:
    azureSubscription: 'MyServiceConnection'
    KeyVaultName: 'MyVault'
    SecretsFilter: '*'
```  
---
#### **6. What is Managed Identity?**  
Managed Identity provides **secure authentication** for Azure services without storing credentials.  
- **System-assigned MI** → Bound to a single Azure resource.  
- **User-assigned MI** → Can be shared across multiple resources.  
---
#### **7. What is Helm Chart and Its Use?**  
Helm is a **package manager** for Kubernetes. Helm charts help deploy applications easily.  
Example **Helm Chart Deployment**:  
```shell
helm install my-app ./my-chart
```
---
#### **8. How to Design a High-Availability (HA) Kubernetes Environment?**  
- **Multiple replicas of pods** (ReplicaSet, Deployment).  
- Use **multiple nodes** across **Availability Zones**.  
- **Load Balancers** & **Ingress Controllers**.  
- Use **Persistent Volumes (PV) & StatefulSets** for data persistence.  
---
### **Azure DevOps & Git**  
#### **9. Azure Template: Using One Pipeline Code in Another**  
Use `extends` in YAML:  
```yaml
extends:
  template: common-template.yaml
  parameters:
    env: production
```  
---
#### **10. Git Pull vs Fetch**  
- `git fetch` → Updates local references but does not modify working directory.  
- `git pull` → Fetches changes and **merges them** into your branch.  
---
#### **11. Git Rebase**  
`git rebase` moves a branch’s commits **on top of another branch**.  
```shell
git checkout feature-branch
git rebase main
```  
---
### **Secret Management**  
#### **12. Where Do You Manage Secrets: Jenkins vs Azure?**  
- **Azure Key Vault** → For Azure DevOps pipelines.  
- **Jenkins Credentials Plugin** → For Jenkins.  
---
### **Variables & Secrets**  
#### **13. Variables and Secrets Usage**  
- **Variables** store environment/config data.  
- **Secrets** store sensitive data (e.g., passwords).  
Example in Azure DevOps:  
```yaml
variables:
  ENV: production
```  
Example Secret Access:  
```yaml
- task: AzureKeyVault@1
  inputs:
    KeyVaultName: 'MyVault'
```  
---
### **14. Single Quotes (' ') vs Double Quotes (" ")**  
- `'Single Quotes'` → Used for **literal values** (No variable expansion).  
- `"Double Quotes"` → Allows **variable expansion** in scripting.  

Example in **Bash**:  
```sh
VAR="Hello"
echo '$VAR'  # Prints: $VAR
echo "$VAR"  # Prints: Hello
```  
---
Would you like me to refine any answers or provide practical examples? 🚀
