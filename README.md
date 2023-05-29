# Step by Step:
1. Fill Region Name, Access Key and Secret Key with Your own Configuration.
2. After confirmed there's no error in Terraform file, Then Run Cilist Terraform file with Command:
# "terraform init"
# "terraform apply"
3. So we got the EC2 Instance IP for React App Backend URL and RDS Endpoint for DB Host.
4. Apply it in Depok and Sydney yml files.
5. Then Run Ansible with Running Command:
# "ansible-playbook -i hosts main.yml"
