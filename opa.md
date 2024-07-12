Install OPA Gatekeeper in the cluster
Let us install OPA Gatekeeper pre-build image using the following command
kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/master/deploy/gatekeeper.yaml

Using OPA Gatekeeper ensure that only whitelisted images are deployed into the cluster
Let us now create a constraint template and apply it to our cluster


cat >> constraint-template.yaml <<EOL
apiVersion: templates.gatekeeper.sh/v1
kind: ConstraintTemplate
metadata:
  name: enforcetrustedimages
spec:
  crd:
    spec:
      names:
        kind: enforcetrustedimages
      validation:
        openAPIV3Schema:
          type: object
          properties:
            images:
              type: array
              items:
                type: string
  targets:
    - target: admission.k8s.gatekeeper.sh
      rego: |
        package enforcetrustedimages

        trusted_images = {images |
            images = input.parameters.images[_]
        }

        images_trusted(str, patterns) {
            image_matches(str, patterns[_])
        }

        image_matches(str, pattern) {
            contains(str, pattern)
        }
        violation[{"msg": msg}] {
          input.review.object
          image := input.review.object.spec.containers[_].image
          name := input.review.object.metadata.name
          not images_trusted(image, trusted_images)
          msg := sprintf("pod %q has an image that is not trusted by your organization %q. Follow the trusted  images %v", [name, image, trusted_images])
        }
EOL


In this constraint template, we first define the schema for our constraint in the openAPIV3Schema section.
In targets we define that this is admission controller.
We then write our Policy in Rego Policy language. The Rego code is simple to understand and much like english statement.
In trusted_images we store all our trusted images and images_trusted function returns true or false if the container image is found in the trusted images.
Let us now create our Constraints to apply our Policy.

Let us now create Constraint

Now, lets create a new constraint file



cat >> constraint.yaml <<EOL
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: enforcetrustedimages
metadata:
  name: enforcetrustedimages
spec:
  match:
    kinds:
      - apiGroups: [""]
        kinds: ["Pod"]
    namespaces:
      - default
  parameters:
    images:
      - hysnsec/safety
      - busybox
      - nginx:latest
EOL



Let us now apply the ConstraintTemplates and Constraints in our Kubernetes cluster.

kubectl apply -f constraint-template.yaml
kubectl apply -f constraint.yaml
kubectl run nginx-opa-test --image=nginx



As you can see our pod as not created. Let us use the nginx:latest image and create a pod.

kubectl run nginx-opa-test --image=nginx:latest

Write top 3 advantages of using OPA Gatekeeper project.
OPA Gatekeeper is an open-source policy engine that helps enforce policies and security measures in Kubernetes clusters. Here are the top three benefits of using OPA Gatekeeper:

Policy Enforcement: With OPA Gatekeeper, you can define policies to ensure that resources in your Kubernetes cluster are deployed according to best practices, regulatory compliance, and security requirements. It provides a framework for defining, validating, and enforcing policies across all Kubernetes objects, including pods, services, and deployments.
Automated Governance: OPA Gatekeeper can automatically enforce policies and prevent unauthorized actions, reducing the burden on administrators and ensuring that all resources are deployed according to policy. It can also automatically remediate any non-compliant resources to ensure that they adhere to policy.
Improved Security: OPA Gatekeeper helps improve the security of Kubernetes clusters by enforcing policies that ensure that only authorized users can access resources, and that all resources are deployed according to security best practices. It can also detect and prevent malicious activities, such as privilege escalation and data exfiltration.
