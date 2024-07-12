

Challenge 1 (25 points)
Ensure trusted images are deployed in the Kubernetes Cluster

Install OPA gatekeeper in the cluster
Using OPA gatekeeper ensure that only whitelisted images are deployed into the cluster
Write top 3 advantages of using OPA gatekeeper project.
Challenge 2 (25 points)
Monitor Runtime security of your mysql workload using kubearmor.

Ensure system binary folders(/bin, /sbin, /usr/sbin/, /usr/bin) are readable only.
Ensure that you block access to k8s service account token
Alert on all rmdir syscalls targeting anything in /var/ directory and sub-directories



###Install OPA gatekeeper in the cluster

kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/release-3.7/deploy/gatekeeper.yaml
kubectl get pods -n gatekeeper-system


### Define a Constraint Template for Whitelisted Images

apiVersion: templates.gatekeeper.sh/v1beta1
kind: ConstraintTemplate
metadata:
  name: k8sallowedrepos
spec:
  crd:
    spec:
      names:
        kind: K8sAllowedRepos
  targets:
    - target: admission.k8s.gatekeeper.sh
      rego: |
        package k8sallowedrepos

        violation[{"msg": msg}] {
          input.review.kind.kind == "Pod"
          container := input.review.object.spec.containers[_]
          not startswith(container.image, "whitelisted-registry.com/")
          msg := sprintf("Container image '%v' is not from a whitelisted registry.", [container.image])
        }

########mettre dans un fichier yaml et créer


###  Create a Constraint to Enforce the Policy


apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sAllowedRepos
metadata:
  name: allowedrepos
spec:
  match:
    kinds:
      - apiGroups: [""]
        kinds: ["Pod"]
  parameters:
    repos:
      - "whitelisted-registry.com/"

########mettre dans un fichier yaml et créer

Top 3 Advantages of Using OPA Gatekeeper
Enhanced Security and Compliance:

OPA Gatekeeper allows for fine-grained control over the resources being deployed in your Kubernetes cluster. By defining policies that ensure only whitelisted images are used, it helps in maintaining security and compliance standards, preventing the use of unauthorized or potentially vulnerable images.
Policy as Code:

With OPA Gatekeeper, policies are defined as code using Rego, a declarative policy language. This approach enables versioning, auditing, and collaboration through standard DevOps practices. Policies can be reviewed, tested, and managed alongside application code, ensuring consistency and reliability.
Extensibility and Integration:

OPA Gatekeeper is highly extensible and can integrate with various tools and platforms in the Kubernetes ecosystem. It supports custom policies for a wide range of use cases, from resource quotas and naming conventions to security best practices and operational guidelines, making it a versatile solution for policy enforcement in Kubernetes environments.
