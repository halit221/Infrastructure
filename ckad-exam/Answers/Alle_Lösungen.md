Hier sind alle **Lösungen zu den 16 CKAD-Aufgaben** im Überblick (kompakt & prüfungsorientiert):

---

### **1. Pods & Labels**
1. **Pod erstellen**  
   ```sh
   kubectl run nginx-pod --image=nginx:alpine --labels=app=webserver
   ```
2. **Logs exportieren**  
   ```sh
   kubectl logs nginx-pod > /tmp/nginx-pod.log
   ```

### **2. Deployments**
3. **Deployment erstellen**  
   ```sh
   kubectl create deployment web-app --image=nginx:1.23 --replicas=3
   kubectl label pods -l app=web-app tier=frontend
   ```
4. **Rolling Update**  
   ```sh
   kubectl set image deployment/web-app nginx=nginx:1.24 --record
   ```

### **3. Services & Networking**
5. **ClusterIP-Service**  
   ```sh
   kubectl expose deployment web-app --name=web-service --port=80 --type=ClusterIP
   ```
6. **Port-Forwarding**  
   ```sh
   kubectl port-forward <pod-name> 8080:80
   ```

### **4. ConfigMaps & Secrets**
7. **ConfigMap als Env-Var**  
   ```yaml
   env:
   - name: APP_COLOR
     valueFrom:
       configMapKeyRef:
         name: app-config
         key: APP_COLOR
   ```
8. **Secret als Env-Var**  
   ```sh
   kubectl create secret generic db-secret --from-literal=DB_USER=admin --from-literal=DB_PASS=secret
   ```

### **5. Multi-Container Pods**
9. **Sidecar-Container**  
   ```yaml
   volumes:
   - name: logs
     emptyDir: {}
   containers:
   - name: nginx
     volumeMounts:
     - name: logs
       mountPath: /var/log/nginx
   - name: sidecar
     image: busybox
     args: [tail, -f, /logs/access.log]
     volumeMounts:
     - name: logs
       mountPath: /logs
   ```

### **6. Probes**
10. **Liveness Probe**  
    ```yaml
    livenessProbe:
      httpGet:
        path: /healthz
        port: 80
      initialDelaySeconds: 5
      periodSeconds: 5
    ```

### **7. Jobs & CronJobs**
11. **Job**  
    ```sh
    kubectl create job data-processor --image=busybox -- /bin/sh -c "echo Processing data..."
    ```
12. **CronJob**  
    ```sh
    kubectl create cronjob backup-job --image=busybox --schedule="*/10 * * * *" -- /bin/sh -c "echo Backup done!"
    ```

### **8. Debugging**
13. **Fehleranalyse**  
    ```sh
    kubectl describe pod broken-pod
    kubectl logs broken-pod
    ```
14. **Ressourcenlimits**  
    ```yaml
    resources:
      limits:
        memory: "100Mi"
        cpu: "500m"
    ```

### **9. Imperative Kommandos**
15. **Temporärer Pod**  
    ```sh
    kubectl run temp-pod --image=busybox --restart=Never -- /bin/sh -c "sleep 3600"
    ```

---

### **Prüfungstipps:**
1. **Alias setzen**: `alias k=kubectl` spart Zeit.
2. **Imperativ > Deklarativ**: Nutze `--dry-run=client -o yaml` für YAML-Vorlagen.
3. **Dokumentation**: `kubectl explain pod.spec.containers` während der Prüfung nutzen.

**Weiterführend**: Übe besonders **Rolling Updates**, **Multi-Container Pods** und **Debugging** – diese sind Prüfungsschwerpunkte! 🚀