Ensure system binary folders (/bin, /sbin, /usr/sbin/, /usr/bin) are readable only
First, we need to install the Karmor command line tool in order to use it for installing KubeArmor.

wget -qO- https://github.com/kubearmor/kubearmor-client/releases/download/v1.2.0/karmor_1.2.0_linux_amd64.tar.gz | tar xvz -C /usr/local/bin/
chmod +x /usr/local/bin/karmor 
karmor install

Next, we will create a MySQL pod.

cat > mysql-pod.yaml <<EOL
apiVersion: v1
kind: Pod
metadata:
  name: mysql-pod
  labels:
    name: mysql-pod
spec:
  containers:
    - name: mysql
      image: mysql:latest
      env:
        - name: "MYSQL_USER"
          value: "mysql"
        - name: "MYSQL_PASSWORD"
          value: "mysql"
        - name: "MYSQL_DATABASE"
          value: "sample"
        - name: "MYSQL_ROOT_PASSWORD"
          value: "supersecret"
      ports:
        - containerPort: 3306
EOL

kubectl create -f mysql-pod.yaml

After creating the pod, we will proceed to create a policy that will check for File Integrity Monitoring.



cat > policy1.yaml <<EOL
apiVersion: security.kubearmor.com/v1
kind: KubeArmorPolicy
metadata:
  name: fim-for-system-paths
  namespace: default
spec:
  action: Block
  file:
    matchDirectories:
    - dir: /bin/
      readOnly: true
      recursive: true
    - dir: /sbin/
      readOnly: true
      recursive: true
    - dir: /usr/sbin/
      readOnly: true
      recursive: true
    - dir: /usr/bin/
      readOnly: true
      recursive: true
  message: Alert! An attempt to write to system directories denied.
  severity: 5
  tags:
  - NIST
  - PCI-DSS

EOL

kubectl create -f policy1.yaml

kubectl exec -it mysql-pod -- bash
touch /usr/sbin/test

We were unable to create any files inside the /usr/sbin/ directory. Please remember to exit from the pod.

exit




#######"Ensure that you block access to Kubernetes service account token
In order to prevent access to a Kubernetes service account, we need to block anyone from accessing the directory /run/secrets/kubernetes.io/serviceaccount/.

Here are the policies you can use to achieve this.


cat > policy2.yaml <<EOL
apiVersion: security.kubearmor.com/v1
kind: KubeArmorPolicy
metadata:
  name: ksp-block-sa
  namespace: default
spec:
  selector:
    matchLabels:
      name: mysql-pod
  file:
    matchDirectories:
    - dir: /run/secrets/kubernetes.io/serviceaccount/
      recursive: true
  action:
    Block
EOL



kubectl create -f policy2.yaml
kubectl exec -it mysql-pod -- bash
cat /run/secrets/kubernetes.io/serviceaccount/token


We are receiving a Permission denied error. Have you considered calling the Kubernetes API?

curl https://$KUBERNETES_PORT_443_TCP_ADDR/api --insecure --header "Authorization: Bearer $(cat /run/secrets/kubernetes.io/serviceaccount/token)"

exit


#####Task4

Let’s create a policy that prevents the deletion of any directory within the /var/ directory.


cat > policy3.yaml <<EOL
apiVersion: security.kubearmor.com/v1
kind: KubeArmorPolicy
metadata:
  name: audit-var-rmdir
  namespace: default
spec:
  selector:
    matchLabels:
      name: mysql-pod
  syscalls:
    matchPaths:
    - syscall:
      - rmdir
      path: /var/
      recursive: true
  action:
    Audit
  message: Danger! Attempt to delete file under var directory.
EOL


kubectl create -f policy3.yaml\
Once done, you need to open a new terminal. To do this, click on the + icon located in the top left corner of the terminal. This will open a new terminal window called 2nd Terminal.

In this terminal, you can execute the command karmor logs to view real-time logs from KubeArmor and verify that our policy is functioning correctly.

karmor logs

The current output is not displaying any information. Please go to the 1st Terminal and try to delete a directory within the /var/ directory.



kubectl exec -it mysql-pod -- bash
rmdir /var/games
