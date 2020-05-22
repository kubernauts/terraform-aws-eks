# Deploy EKS with Spot Instances Support

git clone https://github.com/arashkaffamanesh/terraform-aws-eks.git

cd base

```bash
terraform init
terraform plan
terraform apply
terraform enjoy :-)
```

You should get an EKS Cluster with 1 spot instance.

## Changes which was needed

If you'd like to clone the upstream module, do the following:

git clone https://github.com/terraform-aws-modules/terraform-aws-eks.git

cp -r examples/spot_instances base

cd base

in main.tf of the base folder change the

module "eks" {
  source       = "../.."

to

module "eks" {
  source       = "../"

Change the region in variables.tf to:

variable "region" {
  default = "eu-central-1"
}

Change the instance types in main.tf to:

override_instance_types = ["t3a.medium", "t3.medium"]

```bash
worker_groups_launch_template = [
    {
      name                    = "spot-1"
      override_instance_types = ["t3a.medium", "t3.medium"]
      spot_instance_pools     = 2
      asg_max_size            = 1
      asg_desired_capacity    = 1
      kubelet_extra_args      = "--node-labels=node.kubernetes.io/lifecycle=spot"
      public_ip               = true
    },
  ]
```

## Deploy

```bash
terraform init
terraform plan
terraform apply
terraform enjoy :-)
```
