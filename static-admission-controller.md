#
Static Admission Controllers
Static Admission Controllers
A static admission controller in Kubernetes is a type of admission controller that is built-in to the Kubernetes API server and runs as part of the API server’s 
request handling pipeline. It is responsible for validating and mutating incoming API requests before they are processed by the Kubernetes API server.

Since these admission controllers are built in they are called static admission controllers.
The static admission controller can also be used to validate and mutate incoming requests to ensure that they conform to specific policies and security standards.

Some examples of static admission controllers in Kubernetes are

DefaultStorageClass
AlwaysPullImages
NamespaceLifecycle
and many more.
For complete list please refer to Kubernetes documentation https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/

Let us now look at couple of static admission controllers





###
AlwaysPullImages
The AlwaysPullImages admission controller modifies every new Pod to force the image pull policy to Always. 
This means that the image will always be pulled from the registry before starting the container,
regardless of whether the image already exists locally or not. 
This ensures that the most recent version of the image is used. When this admission controller is enabled,
images are always pulled prior to starting containers, which means valid credentials are required.

cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: nginx-image-pull-2
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    imagePullPolicy: Always
    ports:
    - containerPort: 80


DefaultStorageClass
The DefaultStorageClass admission controller observes creation of PersistentVolumeClaim objects that do not request any specific storage class and automatically adds a 
default storage class to them. When a Persistent Volume Claim (PVC) is created without specifying a storage class, the PVC is automatically bound to the default storage class. 
This means that if a pod in the cluster requests a certain amount of storage, and no specific storage class is specified, the pod will use the default storage class.
The default storage class can be configured and changed by cluster administrators.

Create Persistent Volume
A Persistent Volume (PV) is a piece of storage that has been provisioned and made available to a cluster. PVs can be backed by a variety of storage solutions, 
such as local disks, NFS shares, or cloud-based storage services like AWS Elastic Block Store (EBS) or Google Persistent Disks. PVs are independent of pods and nodes,
and can be used by one or many pods at the same time. Pods can consume the storage by creating a Persistent Volume Claim (PVC), which is a request for a specific amount 
of storage from a PV. Once a PVC is created, the Kubernetes control plane automatically binds the PVC to a PV, making the storage available to the pod.


LimitRanger
The LimitRanger admission controller is a Kubernetes feature that allows administrators to set resource limits on pods in a namespace. It ensures that pods are not created in a namespace if they would exceed the resource limits set for that namespace. The resource limits that can be set include CPU and memory limits. This feature can be useful for ensuring that a namespace does not consume too many resources, preventing other namespaces from functioning properly. We will cover Limit Ranger in more detail in Defending Kubernetes Cluster module. For now let us create a LimitRange object using the below manifest file.


cat > limit-range.yaml <<EOL
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-limit-range
spec:
  limits:
  - default:
      cpu: 500m
    max: # max and min define the limit range
      cpu: 800m
    min:
      cpu: 100m
    type: Container

In the above manifest we define LimitRange object that specifies default CPU limit and max and min CPU limit. Whenever the pod is created in the default namespace, 
the LimitRanger admission controller will make sure that pods has the default Limit set. let us try that out

In this chapter we looked at three static admission controllers. DefaultStorageClass, AlwaysPullImages and LimitRanger. These admission controllers are available
by default and it is recommended to use them. There are other static admission controllers available in Kubernetes. If you feel like trying out few more admission
controller please visit kubernetes documentation https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers
