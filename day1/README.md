# Installation Steps (Running your first app on Kubernetes)

1. Install Colima by following the instructions at [Colima GitHub repository](https://github.com/abiosoft/colima/).
2. Start Colima with Kubernetes support enabled by running the command: `colima start --kubernetes`.
3. Create a Docker registry secret using the following command:
   ```bash
   kubectl create secret docker-registry my-registry-secret \
       --docker-server=https://index.docker.io/v1/ \
       --docker-username=vivekprakashsingh \
       --docker-password=dckr_pat_*******************
   ```
4. Deploy your Kubernetes resources by applying the configurations in the YAML files:
   - Apply the Pod configuration: `kubectl apply -f k8s-pod.yaml`.
   - Apply the Service configuration: `kubectl apply -f k8s-service.yaml`.
5. Retrieve the IP address and NodePort for accessing your service:
   - Run: `kubectl get endpoints` and copy the IP address of Kubernetes (e.g., 192.168.106.2).
   - Run: `kubectl get service` and copy the NodePort for the `k8s-service`.
6. Open your browser and navigate to the IP address and NodePort combination, e.g., `192.168.106.2:30436`.
7. Congratulations! Your first service is now live on Kubernetes.
