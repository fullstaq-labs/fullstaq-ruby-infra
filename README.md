# Fullstaq Ruby infrastructure

This repository codifies the Fullstaq Ruby infrastructure. You need Terraform, kubectl and the Google Cloud CLI.

## Initial deployment

Create the `fullstaq-ruby` project in Google Cloud. And within that project, create a Google Cloud Storage bucket for storing the Terraform state.

Next, make sure the Google Cloud CLI is properly logged in (both using normal auth and application default auth), set to the right project, and enable the required Google Cloud APIs:

~~~bash
gcloud auth application-default login
gcloud auth login
gcloud config set project fullstaq-ruby
../enable_gcloud_apis
~~~

Next, create `terraform/backend.tf` using the example file. Then initialize Terraform:

~~~bash
cd terraform
terraform init -backend-config=backend.tf
~~~

Finally, create the infrastructure:

~~~bash
terraform apply
~~~

Terraform would have created a Kubernetes cluster. Pass its connection credentials to kubectl:

~~~bash
gcloud container clusters get-credentials fullstaq-ruby --zone us-east1-c --project fullstaq-ruby
~~~

Then apply its Kustomization file:

~~~bash
kubectl apply -k ../kubernetes
~~~

## Updates

These steps assume that your Google Cloud CLI is properly logged in and set to the right project, that your Terraform is initialized, and that your kubectl is authenticated to the Kubernetes cluster.

~~~bash
cd terraform
terraform apply
kubectl apply -k ../kubernetes
~~~
