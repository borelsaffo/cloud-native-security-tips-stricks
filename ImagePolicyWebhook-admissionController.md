#
ImagePolicyWebhook is a builtin admission controller that can be used to apply specific policies on images. 
The ImagePolicyWebhook admission controller is not enabled by default. We need to enable this admission controller in the API server.
This can be done by passing the flag to Kube API server like this
--enable-admission-plugins=NodeRestriction,ImagePolicyWebhook We will do this in next step.
The ImagePolicyWebhook depends on the external service and we have to make sure that this service is up in order to use the image policies.
The good thing about ImagePolicyWebhook is that if the external service is down, then we also have an option to instruct API server to accept images. 
We will be simulating this scenario in this exercise. We will configure ImagePolicyWebhook up to a point where it contacts the external service.
Create Admission Configuration
ImagePolicyWebhook uses a configuration file to set options for the behavior of the backend. This could be in JSON format or YAML format. We will use the YAML format and create the file.
We will first create a folder to store all the files related to ImagePolicyWebhook

mkdir /etc/kubernetes/admission/
##### Configuration File

cat > /etc/kubernetes/admission/admission-configuration.yaml <<EOL
apiVersion: apiserver.config.k8s.io/v1
kind: AdmissionConfiguration
plugins:
  - name: ImagePolicyWebhook
    configuration:
      imagePolicy:
        kubeConfigFile: /etc/kubernetes/admission/config
        allowTTL: 50
        denyTTL: 50
        retryBackoff: 500
        defaultAllow: false

The above configuration looks for kubeConfig file located at etc/kubernetes/admission/config.
We will create that in a moment. We also pass more configurations values like allowTTL and denyTTL.
The defaultAllow: false will instruct the API server not to allow images if the webhook server is down or not reachable.

#### Create KubeConfig File
Let us now create a KubeConfig file.
cat > /etc/kubernetes/admission/config <<EOL
apiVersion: v1
kind: Config

# clusters refers to the remote service.
clusters:
- cluster:
    certificate-authority: /etc/kubernetes/admission/external-cert.pem  # CA for verifying the remote service.
    server: https://my-external-service:1111/image-policy    # URL of remote service to query. Must use 'https'.
  name: image-policy

contexts:
- context:
    cluster: image-policy
    user: api-server
  name: image-policy
current-context: image-policy
preferences: {}

# users refers to the API server's webhook configuration.
users:
- name: api-server
  user:
    client-certificate: /etc/kubernetes/admission/apiserver-client-cert.pem  # cert for the webhook admission controller
    client-key:  /etc/kubernetes/admission/apiserver-client-key.pem          # key matching the cert
EOL

In the above kube config file we specify the external service server: https://my-external-service:1111/image-policy
In the users section we pass the API servers webhook configuration details.

The webhook endpoint must be secured by tls to be used by kubernetes. This certificate can be a self-signed one. 
We will use the following command to create a self signed certificate for the external service


openssl req  -nodes -new -x509 -keyout external-cert-key.pem -out external-cert.pem
mv external-cert-key.pem /etc/kubernetes/admission/
mv external-cert.pem /etc/kubernetes/admission/
openssl req  -nodes -new -x509 -keyout apiserver-client-key.pem  -out apiserver-client-cert.pem
mv apiserver-client-key.pem /etc/kubernetes/admission/
mv apiserver-client-cert.pem /etc/kubernetes/admission/

#### Configure Kube API server
We now need to enable ImagePolicyWebhook in Kube API server and also pass in the required configuration.

Before doing that let us take the backup of our kube apiserver manifest file.

cp /etc/kubernetes/manifests/kube-apiserver.yaml /kube-apiserver-bak.yaml
nano /etc/kubernetes/manifests/kube-apiserver.yaml
- --admission-control-config-file=/etc/kubernetes/admission/admission-configuration.yaml
- 
##### NOTE: enable-admission-plugins may already be present in the kube API server manifest file.
In that case you need to append ImagePolicyWebhook to the list of admission plugins.
After adding the above two lines the kube api server manifest must look as below
![image](https://github.com/borelsaffo/cloud-native-security-tips-stricks/assets/27947973/4504c4ee-629b-42a2-ac3d-f6dd916068f6)


  - mountPath: /etc/kubernetes/admission
    name: admission
    readOnly: true
- hostPath:
    path: /etc/kubernetes/admission
  name: admission

cat > /etc/kubernetes/admission/admission-configuration.yaml <<EOL
apiVersion: apiserver.config.k8s.io/v1
kind: AdmissionConfiguration
plugins:
  - name: ImagePolicyWebhook
    configuration:
      imagePolicy:
        kubeConfigFile: /etc/kubernetes/admission/config
        allowTTL: 50
        denyTTL: 50
        retryBackoff: 500
        defaultAllow: true

If you notice we added defaultAllow: true which means that if the external service is not up then we will be able to download the image that we specify.
For this to work we need to restart the kube api server because we made configuration changes and kube api server needs to pick the latest configuration.

One of the easy way to restart the api server is to rename the current api server config file to something else, and then rename the api server config file to its original name.

Let’s start by renaming the kube-apiserver.yaml to kube-apiserver-original.yaml.


##### Conclusion

In this exercise we saw how the ImagePolicyWebhook admission controller works.

We did not use a real webhook server, but we did see the configuration involved in making ImagePolicyWebhook work.

The CA and the certs used in this demo are randomly generated, but you can use to choose to use your CA verts in your deployments.

The webhook server we used in this exercise will never be reachable, and hence you were not able to create a pod.
The main goal of this exercise was to review what it takes to enable ImagePolicyWebhook, how to use it’s configurations.

If you want to challenge yourself and use a real server you can try that from the link https://github.com/flavio/kube-image-bouncer.
