# aws-ecr-ci-cd

GITHUB ACTIONS/PUSH DOCKER IMAGE TO ECR AND DEPLOYING EKS - TWILIO WHATSAPP INTEGRATION

GitHub Actions Workflow to deploy automatically a Docker image into a ECR Repository in AWS, also, a twilio integration for sending messages to WhatsApp

1. You need to have created a certain things before:

- Create a cluster to deploy the nginx app (app1 folder)
- ECR repository
- Create and OIDC Provider in IAM and associate with a new role called github-oidc-auth-user/
    This is neccessry, because It should match with the name role wich assume the GitHub Actions
- To the Role created in the previous step, you have to give it the necessary permissions, for EKS and ECR
- RBAC Permissions to access Api in the cluster

---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: github-oidc-cluster-role
rules:
  - apiGroups: ["*"]
    resources: ["*"]
    verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]

---
apiVersion: rbac.authorization.k8s.io/v1
# This cluster role binding allows user "github-oidc-auth-user" to perform any operation in any namespace.
kind: ClusterRoleBinding
metadata:
  name: github-oidc-cluster-role-binding
subjects:
  - kind: User
    name: github-oidc-auth-user # Name is case sensitive
    apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: github-oidc-cluster-role
  apiGroup: rbac.authorization.k8s.io

- Create this secrets in GitHub Actions:
    TWILIO_ACCOUNT_SID:
    TWILIO_AUTH_TOKEN
    WHATSAPP_NUMBER

