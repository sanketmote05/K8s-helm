# K8s-helm

Helm chart exercise that introduces students to deploying a Kubernetes application using Helm.

---

### **Exercise: Deploying a Kubernetes Application using Helm**

#### **Objective:**
Learn how to create, customize, and deploy a Helm chart for a sample web application on a Kubernetes cluster.

#### **Prerequisites:**
1. Minikube or any Kubernetes cluster (students can use Minikube if they have it set up).
2. Helm installed on the local machine.
3. Basic knowledge of Kubernetes and Helm.

---

### **Step 1: Install Minikube and Helm**

Ensure Minikube is running:

```bash
minikube start
```

Check if Helm is installed, or install it if not:

```bash
helm version
```

If Helm is not installed, follow the [Helm Installation Guide](https://helm.sh/docs/intro/install/) to set it up.

---

### **Step 2: Create a Sample Helm Chart**

Create a new Helm chart:

```bash
helm create mywebapp
```

This command will create a new directory named `mywebapp` containing all the default files and directories for a Helm chart, including templates and values files.

---

### **Step 3: Modify the Default Chart**

1. **Update the `values.yaml`** file to customize the chart:
   
   In `mywebapp/values.yaml`, replace the default values with the following configuration:

   ```yaml
   replicaCount: 2

   image:
     repository: nginx
     pullPolicy: IfNotPresent
     tag: "1.19.2"

   service:
     type: NodePort
     port: 80

   ingress:
     enabled: false

   resources:
     limits:
       cpu: "100m"
       memory: "128Mi"
     requests:
       cpu: "100m"
       memory: "64Mi"
   ```

2. **Edit the `deployment.yaml` template:**

   Go to `mywebapp/templates/deployment.yaml` and modify the `nginx` container resources as defined in `values.yaml`. Ensure that the CPU and memory requests/limits are set correctly for the deployment.

---

### **Step 4: Install the Chart**

1. First, package and install the chart on the Kubernetes cluster:

   ```bash
   helm install my-release ./mywebapp
   ```

2. Verify that the chart has been deployed:

   ```bash
   kubectl get pods
   kubectl get svc
   ```

3. Access the web application using the `NodePort` service:

   ```bash
   minikube service my-release-mywebapp
   ```

   This command will open the default NGINX web page in your browser.

---

### **Step 5: Customize the Chart (Optional)**

1. **Enable Ingress**: In `values.yaml`, set `ingress.enabled` to `true` and configure the ingress hostname and path.

2. **Override Values**: Try overriding values during installation:

   ```bash
   helm install my-release ./mywebapp --set service.type=LoadBalancer --set replicaCount=3
   ```

3. **Upgrade the Release**: Make changes to your chart and upgrade the release:

   ```bash
   helm upgrade my-release ./mywebapp
   ```

4. **Rollback the Release**: Rollback to a previous version if necessary:

   ```bash
   helm rollback my-release 1
   ```

---

### **Step 6: Clean Up**

After completing the exercise, clean up the resources:

```bash
helm uninstall my-release
minikube stop
```

---

### **Exercise Completion**

By the end of this exercise, you will have:
- Created and deployed a Helm chart for an NGINX web application.
- Customized the Helm chart using `values.yaml`.
- Managed the application release by installing, upgrading, and rolling back changes.

---

### **Bonus: Create Your Own Helm Chart**

Try creating a Helm chart from scratch for a different application (e.g., a Python or Node.js web app) and repeat the process.

---

This exercise provides a practical introduction to using Helm to manage Kubernetes applications, with room for students to experiment and extend their knowledge.
