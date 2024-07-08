# Install KubiScan
KubiScan is a tool for scanning Kubernetes cluster for risky permissions in Kubernetes’s Role-based access control (RBAC) authorization model.

First, download the KubiScan package using the wget command:

wget https://github.com/cyberark/KubiScan/archive/refs/tags/v1.6.zip

apt update && apt install unzip -y

unzip v1.6.zip

cd KubiScan-1.6

pip3 install -r requirements.txt

alias kubiscan='python3 ~/KubiScan-1.6/KubiScan.py'

kubiscan -h

kubiscan -a

![image](https://github.com/borelsaffo/cloud-native-security-tips-stricks/assets/27947973/4a5fb55c-add9-458d-b4f4-45f78938bcec)

![image](https://github.com/borelsaffo/cloud-native-security-tips-stricks/assets/27947973/91fd1224-3576-4721-8504-3b42c989c997)

/KubiScan-1.6# kubiscan -a

                   `-/osso/-`                    
                `-/osssssssssso/-`                
            .:+ssssssssssssssssssss+:.            
        .:+ssssssssssssssssssssssssssss+:.        
     :osssssssssssssssssssssssssssssssssssso:     
    /sssssssssssss+::osssssso::+sssssssssssss+    
   `sssssssssso:--..-`+ssss+ -..--:ossssssssss`   
   /sssssssss:.+ssss/ /ssss/ /ssss+.:sssssssss/   
  `ssssssss:.+sssssss./ssss/`sssssss+.:ssssssss`  
  :ssssss/`-///+oss+/`-////-`/+sso+///-`/ssssss/  
  sssss+.`.-:-:-..:/`-++++++-`/:..-:-:-.`.+sssss` 
 :ssso..://:-`:://:.. osssso ..://::`-://:..osss: 
 osss`-/-.`-- :.`.-/. /ssss/ ./-.`-: --`.-/-`osso 
-sss:`//..-`` .`-`-//`.----. //-`-`. ``-..//.:sss-
osss:.::`...`- ..`.:/`+ssss+`/:``.. -`...`::.:ssso
+ssso`:/:`--`:`--`/:-`ssssss`-//`--`:`--`:/:`osss+
 :sss+`-//.`...`-//..osssssso..//-`...`.//-`+sss: 
  `+sss/...::/::..-+ssssssssss+-..::/::.../sss+`  
    -ossss+/:::/+ssssssssssssssss+/:::/+sssso-    
      :ssssssssssssssssssssssssssssssssssss/      
       `+ssssssssssssssssssssssssssssssss+`       
         -osssssssssssssssssssssssssssss-         
          `/ssssssssssssssssssssssssss/`       
    
               KubiScan version 1.6
               Author: Eviatar Gerzi
    
Using kube config file.
+----------------------------+
|Risky Roles and ClusterRoles|
+----------+-------------+----------------------+---------------------------------------------+-----------------------------------+
| Priority | Kind        | Namespace            | Name                                        | Creation Time                     |
+----------+-------------+----------------------+---------------------------------------------+-----------------------------------+
| HIGH     | Role        | default              | gitlab                                      | Mon Jul  8 22:45:23 2024 (0 days) |
| CRITICAL | Role        | default              | secret-reader                               | Mon Jul  8 22:45:24 2024 (0 days) |
| CRITICAL | Role        | kube-system          | system:controller:bootstrap-signer          | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | Role        | kube-system          | system:controller:token-cleaner             | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | Role        | kubernetes-dashboard | kubernetes-dashboard                        | Mon Jul  8 22:45:22 2024 (0 days) |
| HIGH     | Role        | test                 | pod-reader                                  | Mon Jul  8 23:08:43 2024 (0 days) |
| CRITICAL | ClusterRole | None                 | admin                                       | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | ClusterRole | None                 | cluster-admin                               | Mon Jul  8 22:45:13 2024 (0 days) |
| CRITICAL | ClusterRole | None                 | edit                                        | Mon Jul  8 22:45:14 2024 (0 days) |
| LOW      | ClusterRole | None                 | system:aggregate-to-admin                   | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | ClusterRole | None                 | system:aggregate-to-edit                    | Mon Jul  8 22:45:14 2024 (0 days) |
| HIGH     | ClusterRole | None                 | system:controller:cronjob-controller        | Mon Jul  8 22:45:14 2024 (0 days) |
| HIGH     | ClusterRole | None                 | system:controller:daemon-set-controller     | Mon Jul  8 22:45:14 2024 (0 days) |
| HIGH     | ClusterRole | None                 | system:controller:deployment-controller     | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | ClusterRole | None                 | system:controller:expand-controller         | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | ClusterRole | None                 | system:controller:generic-garbage-collector | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | ClusterRole | None                 | system:controller:horizontal-pod-autoscaler | Mon Jul  8 22:45:14 2024 (0 days) |
| HIGH     | ClusterRole | None                 | system:controller:job-controller            | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | ClusterRole | None                 | system:controller:namespace-controller      | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | ClusterRole | None                 | system:controller:persistent-volume-binder  | Mon Jul  8 22:45:14 2024 (0 days) |
| HIGH     | ClusterRole | None                 | system:controller:replicaset-controller     | Mon Jul  8 22:45:14 2024 (0 days) |
| HIGH     | ClusterRole | None                 | system:controller:replication-controller    | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | ClusterRole | None                 | system:controller:resourcequota-controller  | Mon Jul  8 22:45:14 2024 (0 days) |
| HIGH     | ClusterRole | None                 | system:controller:statefulset-controller    | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | ClusterRole | None                 | system:kube-controller-manager              | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | ClusterRole | None                 | system:node                                 | Mon Jul  8 22:45:14 2024 (0 days) |
+----------+-------------+----------------------+---------------------------------------------+-----------------------------------+


+------------------------------------------+
|Risky RoleBindings and ClusterRoleBindings|
+----------+--------------------+----------------------+---------------------------------------------+-----------------------------------+
| Priority | Kind               | Namespace            | Name                                        | Creation Time                     |
+----------+--------------------+----------------------+---------------------------------------------+-----------------------------------+
| CRITICAL | ClusterRoleBinding | None                 | cluster-admin                               | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | ClusterRoleBinding | None                 | kubeadm:cluster-admins                      | Mon Jul  8 22:45:15 2024 (0 days) |
| CRITICAL | ClusterRoleBinding | None                 | kubernetes-dashboard                        | Mon Jul  8 22:45:22 2024 (0 days) |
| HIGH     | ClusterRoleBinding | None                 | system:controller:cronjob-controller        | Mon Jul  8 22:45:14 2024 (0 days) |
| HIGH     | ClusterRoleBinding | None                 | system:controller:daemon-set-controller     | Mon Jul  8 22:45:14 2024 (0 days) |
| HIGH     | ClusterRoleBinding | None                 | system:controller:deployment-controller     | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | ClusterRoleBinding | None                 | system:controller:expand-controller         | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | ClusterRoleBinding | None                 | system:controller:generic-garbage-collector | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | ClusterRoleBinding | None                 | system:controller:horizontal-pod-autoscaler | Mon Jul  8 22:45:14 2024 (0 days) |
| HIGH     | ClusterRoleBinding | None                 | system:controller:job-controller            | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | ClusterRoleBinding | None                 | system:controller:namespace-controller      | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | ClusterRoleBinding | None                 | system:controller:persistent-volume-binder  | Mon Jul  8 22:45:14 2024 (0 days) |
| HIGH     | ClusterRoleBinding | None                 | system:controller:replicaset-controller     | Mon Jul  8 22:45:14 2024 (0 days) |
| HIGH     | ClusterRoleBinding | None                 | system:controller:replication-controller    | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | ClusterRoleBinding | None                 | system:controller:resourcequota-controller  | Mon Jul  8 22:45:14 2024 (0 days) |
| HIGH     | ClusterRoleBinding | None                 | system:controller:statefulset-controller    | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | ClusterRoleBinding | None                 | system:kube-controller-manager              | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | ClusterRoleBinding | None                 | system:node                                 | Mon Jul  8 22:45:14 2024 (0 days) |
| HIGH     | RoleBinding        | default              | gitlab                                      | Mon Jul  8 22:45:23 2024 (0 days) |
| CRITICAL | RoleBinding        | default              | secret-reader                               | Mon Jul  8 22:45:24 2024 (0 days) |
| CRITICAL | RoleBinding        | kube-public          | system:controller:bootstrap-signer          | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | RoleBinding        | kube-system          | system:controller:bootstrap-signer          | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | RoleBinding        | kube-system          | system:controller:token-cleaner             | Mon Jul  8 22:45:14 2024 (0 days) |
| CRITICAL | RoleBinding        | kubernetes-dashboard | kubernetes-dashboard                        | Mon Jul  8 22:45:22 2024 (0 days) |
| HIGH     | RoleBinding        | test                 | pod-reader-binding                          | Mon Jul  8 23:08:43 2024 (0 days) |
+----------+--------------------+----------------------+---------------------------------------------+-----------------------------------+


+-----------+
|Risky Users|
+----------+----------------+----------------------+--------------------------------+
| Priority | Kind           | Namespace            | Name                           |
+----------+----------------+----------------------+--------------------------------+
| CRITICAL | Group          | None                 | system:masters                 |
| CRITICAL | Group          | None                 | kubeadm:cluster-admins         |
| CRITICAL | ServiceAccount | kubernetes-dashboard | kubernetes-dashboard           |
| HIGH     | ServiceAccount | kube-system          | cronjob-controller             |
| HIGH     | ServiceAccount | kube-system          | daemon-set-controller          |
| HIGH     | ServiceAccount | kube-system          | deployment-controller          |
| CRITICAL | ServiceAccount | kube-system          | expand-controller              |
| CRITICAL | ServiceAccount | kube-system          | generic-garbage-collector      |
| CRITICAL | ServiceAccount | kube-system          | horizontal-pod-autoscaler      |
| HIGH     | ServiceAccount | kube-system          | job-controller                 |
| CRITICAL | ServiceAccount | kube-system          | namespace-controller           |
| CRITICAL | ServiceAccount | kube-system          | persistent-volume-binder       |
| HIGH     | ServiceAccount | kube-system          | replicaset-controller          |
| HIGH     | ServiceAccount | kube-system          | replication-controller         |
| CRITICAL | ServiceAccount | kube-system          | resourcequota-controller       |
| HIGH     | ServiceAccount | kube-system          | statefulset-controller         |
| CRITICAL | User           | None                 | system:kube-controller-manager |
| HIGH     | ServiceAccount | default              | gitlab                         |
| CRITICAL | ServiceAccount | default              | secret-reader                  |
| CRITICAL | ServiceAccount | kube-system          | bootstrap-signer               |
| CRITICAL | ServiceAccount | kube-system          | token-cleaner                  |
| HIGH     | User           | None                 | bob                            |
+----------+----------------+----------------------+--------------------------------+


+----------------+
|Risky Containers|
+----------+--------------------------------------------+----------------------+---------------------------+-------------------------+----------------------+
| Priority | PodName                                    | Namespace            | ContainerName             | ServiceAccountNamespace | ServiceAccountName   |
+----------+--------------------------------------------+----------------------+---------------------------+-------------------------+----------------------+
| CRITICAL | django-5fb564fd8c-2x4sq                    | default              | django                    | default                 | secret-reader        |
| CRITICAL | dashboard-metrics-scraper-5657497c4c-nwvll | kubernetes-dashboard | dashboard-metrics-scraper | kubernetes-dashboard    | kubernetes-dashboard |
| CRITICAL | kubernetes-dashboard-78f87ddfc-4hx7g       | kubernetes-dashboard | kubernetes-dashboard      | kubernetes-dashboard    | kubernetes-dashboard |
+----------+--------------------------------------------+----------------------+---------------------------+-------------------------+----------------------+
