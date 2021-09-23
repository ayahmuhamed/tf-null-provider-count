# tf-null-provider-count
This repo serves as a learning exercice for the null provider in Terraform.

To ilustrate its usage we'll leverage the count meta-argument and the count.index object attribute (If unfamiliar with count meta-argument, check the official doc [here](https://www.terraform.io/docs/language/meta-arguments/count.html)) to create a couple of ec2 instance on AWS.
Each instance has a null_ressouce definition associated that gather some details of the instance and populate them to a server_details.txt file locally via an exec-local provisioner

## Description of the Terraform null_provider

If unfamiliar with the null_provider, check this [other repo](https://github.com/ayahmuhamed/tf-null-provider) for a short explanation and an introductory example.

## Pre-requirements

* [AWS Account](https://aws.amazon.com/) and basical knowledge of [Terraform with AWS](https://learn.hashicorp.com/collections/terraform/aws-get-started)
* [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed

## How to use this repo

- Clone
- Run
- Cleanup

---

### Clone the repo

```
git clone https://github.com/ayahmuhamed/tf-null-provider-count
```

### Change directory

```
cd tf-null-provider-count
```

### Run


* Initialize Terraform

```
terraform init
```


_Sample output_

```
terraform init

Initializing the backend...

Initializing provider plugins...
- Finding latest version of hashicorp/null...
- Finding latest version of hashicorp/aws...
- Installing hashicorp/aws v3.51.0...
- Installed hashicorp/aws v3.51.0 (signed by HashiCorp)
- Installing hashicorp/null v3.1.0...
- Installed hashicorp/null v3.1.0 (signed by HashiCorp)

# ...
```


* Apply

```
terraform apply
```


_Sample output_

```
terraform apply

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create



Apply complete! Resources: 4 added, 0 changed, 0 destroyed.
```

* Confirm the server_details.txt file has been created locally

```
cat server_details.txt
```

_Sample output_

```
cat server_details.txt
server0, ec2-18-195-166-109.eu-central-1.compute.amazonaws.com, 18.195.166.109
server1, ec2-3-125-120-241.eu-central-1.compute.amazonaws.com, 3.125.120.241
```


* Let's create an additional instance and observe how a new null resource is also created.
For this, run

```
terraform apply -var aws_instance_number=3
```

_Sample output_

```
terraform apply -var aws_instance_number=3
aws_instance.server[1]: Refreshing state... [id=i-0322cb433cc9d0d25]
aws_instance.server[0]: Refreshing state... [id=i-0104ec9535949ada0]
null_resource.null[0]: Refreshing state... [id=5619555824120805796]
null_resource.null[1]: Refreshing state... [id=1016730628362849792]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create


 
``` 



### Cleanup

* Run terraform destroy to delete the created ec2 instances

```
terraform destroy
```

_Sample output_

```
# ...

Plan: 0 to add, 0 to change, 6 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes
  

```
