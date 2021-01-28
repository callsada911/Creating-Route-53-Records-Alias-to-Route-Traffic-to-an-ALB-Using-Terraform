# Creating-Route-53-Records-Alias-to-Route-Traffic-to-an-ALB-Using-Terraform

Introduction
In this hands-on lab, the student will be creating a Route 53 alias record to route traffic from a publicly hosted zone in Route 53 (already provided by A Cloud Guru lab environment) to an application load balancer using Terraform template(s). Please note that changes to Route 53 records may take some time to propagate.

Solution
Log in to the Terraform Controller Node EC2 Instance
Find the details for logging in to the Terraform Controller node provided by the hands-on lab interface and log in to the node using SSH:

ssh cloud_user@<IP-OF-TERRAFORM-CONTROLLER>
Note: This instance already has an EC2 instance profile (role) attached to it and has all necessary AWS API permissions required for this lab. It also has the AWS CLI set up and configured with the AWS account attached to this lab, for which the console login credentials are also provided in the lab interface page once the lab spins up.

After logging in, verify the version of Terraform installed (should be 12.29). Execute the following command to check:

terraform version
Clone the GitHub Repo for Terraform Code
Use the git command to clone the GitHub repo which has the Terraform code for deploying the solution of this lab. GitHub repo URL.

Execute the following command:

git clone https://github.com/linuxacademy/content-deploying-to-aws-ansible-terraform.git
View the resource IDs to use in the next step:

cat resource_ids.txt
Copy all of these values to a notepad file as they will be used later in the lab.

Change to the directory for lab Terraform code:

cd content-deploying-to-aws-ansible-terraform/lab_deploying_dns_acm
Examine the contents of the directory you're in:

ls
Plug in the Provided Resource Values into import_resources.tf
You will need the values for a few pre-configured resources to complete this lab, such as Security Group IDs. These values can be found in the resource_id.txt file in the cloud_user home directory.

Edit the import_resources.tf file to provide the values copied in the previous step:

vim import_resources.tf
Using the values copied in the previous step, replace the "<INSERT-VALUE-HERE>" text for each of the respective values.

Save and quit the file:

:wq
Get Public Hosted Route 53 Zone and Plug It Into variables.tf
A publicly-hosted domain is provided for you as part of this lab and your Terraform controller node has the permissions to make API calls to Route 53 to fetch it.

Carry out the following steps to fetch the domain and plug it into a variable:

Issue the command:

aws route53 list-hosted-zones | jq -r .HostedZones[].Name | egrep "cmcloud*"
This will output a DNS name, ending with a dot.

Copy the DNS value, ensure that you copy the trailing . at the end as well.

Replace it against the default value of the dns-name variable in the variables.tf file:

vim variables.tf
Save and quit the file:

:wq
Deploy the Terraform Code
Initialize the Terraform directory you changed into to download the required provider

terraform init
Ensure Terraform code is formatted properly:

terraform fmt
Ensure code has proper syntax and no errors:

terraform validate
See the execution plan and note the number of resources that will be created:

terraform plan
Deploy resources:

terraform apply
Enter yes when prompted.

After terraform apply has run successfully, you can either use the AWS CLI on the Controller node to list and describe created resources or you can log in to the AWS Console to verify and investigate created resources.

Finally, on the Terraform Controller node CLI, delete all resources which were created and ensure that it runs through successfully.

terraform destroy
