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
