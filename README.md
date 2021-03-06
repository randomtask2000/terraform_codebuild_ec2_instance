![Terraform CodeBuild that Terraforms an EC2 Instance](images/photo-4.jpeg)

# Use Terraform to build a CodeBuild project that runs terraform as a build service
This little repo illustrates how to build a `CodeBuild` project with `terraform` and run terraform inside of this project to build infrastructure from another [repo](https://github.com/randomtask2000/terraform_ec2_instance). This other repo builds a simple `ec2` instance.

## Config
Before running this terraform template add the following terraform config file.

Create a settings file `terraform.auto.tfvars` with the following:
```
echo <<< EOL
aws_access_key = "XXXXXXXXXXXXXXXXXXXX"
aws_secret_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
aws_region = "us-east-1"
public_key = "ssh-rsa AAAAB3NzaC1yc....9wrf+M7Q== my@laptop.local"
vpc_id = "vpc-00000000x00x0xxx0"
terraform_version = "0.9.9"
s3_bucket = "your-s3-bucket-terraform-state"
debug = "true"
EOL >> terraform.auto.tfvars;
```

## terraform apply
After you're done creating the above file and adding your `aws access key`, `secret` and your `ssh public key`, run the following:
```
terraform init
terraform plan
echo yes | terraform apply
```
To remove what you built run:
```
echo yes | terraform destroy
```
The above `terraform apply` statement will create your CodeBuild project. After creating your project in AWS you should find your CodeBase project here:
https://console.aws.amazon.com/codesuite/codebuild/projects?region=us-east-1 
(Change your region in this URL to what you set your `aws_region` variable in your `tfvars` file to.)

## This bit is manual in AWS Console
Your project in CodeBuild will look something like the image below:

![Your CodeBuild project](images/photo-2.jpeg)

Select your project and click `Start Build`: ![Start Build in AWS CodeBuild](images/photo-3.jpeg)

Select `Advanced build overrides`: ![Advanced Build Overrides in AWS CodeBuild](images/photo-5.jpeg)

Unfold `Additional configuration` and edit any of the environment variables. Set the `DESTROY` variable to `true` if you would like to run a build cycle that destroys your infrastructure. ![Advanced Build Overrides in AWS CodeBuild and build environment settings](images/IMG_0265.png)
 
Press `Start Build`.

# And Bob's your uncle
You're done and have fun!
