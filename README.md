# Learn Terraform - Provision an EKS Cluster

This repo is a companion repo to the [Provision an EKS Cluster learn guide](https://learn.hashicorp.com/terraform/kubernetes/provision-eks-cluster), containing
Terraform configuration files to provision an EKS cluster on AWS.# Terraform-And-Kubernetes



1. Install terrafrom CLI

2. Download demo-terraform-eks-cluster.zip file (files already on my repo) cd to the directory

3. Run Terrafrom init

4. Run Terraform apply

5. Configure kubectl

  `aws eks --region $(terraform output -raw region) update-kubeconfig --name $(terraform output -raw cluster_name)`

6. Download the kubernetes metric server tar file

`wget -O v0.3.6.tar.gz https://codeload.github.com/kubernetes-sigs/metrics-server/tar.gz/v0.3.6 && tar -xzf v0.3.6.tar.gz`

7. Unzip the file and apply using kubectl

  `unzip metric-server 0.3.6`


  `kubectl apply -f metrics-server-0.3.6/deploy/1.8+`


==> verify metric server has been deployed


`kubectl get deployment metrics-server -n kube-system`

8. Deploy the kubernetes Dashboard
==> Apply the rbac roles necessary for the dashboard

`kubectl apply -f kubernetes-dashboard-admin.rbac.yaml`

==> Schedule the resources necessary for the dashboard

`kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta8/aio/deploy/recommended.yaml`

==> Verifying the Dashboard resources

`kubectl get pods -n kubernetes-dashboard -o wide`
`kubectl get deployment -n kubernetes-dashboard -o wide`
`kubectl get svc -n kubernetes-dashboard -o wide`


==> Get the url
`kubectl proxy`



9. Authenticate the dashbord

`kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep service-controller-token | awk '{print $1}')`
