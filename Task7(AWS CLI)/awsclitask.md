step1:create vpc using aws cli
aws ec2 create-vpc --cidr-block 10.0.0.0/16 --tag-specifications ResourceType=vpc,Tags=[{Key=Name,Value=MyVpc}]


step2:create public and private subnets using aws cli

aws ec2 create-subnet --vpc-id vpc-081ec835f3EXAMPLE --cidr-block 10.0.0.0/24 --tag-specifications ResourceType=subnet,Tags=[{Key=Name,Value=public-subnet}]


aws ec2 create-subnet
--vpc-id vpc-081ec835f3EXAMPLE
--cidr-block 10.0.0.0/24
--tag-specifications ResourceType=subnet,Tags=[{Key=Name,Value=private-subnet}]

step3:create Internet Gateway
aws ec2 create-internet-gateway --tag-specifications ResourceType=internet-gateway,Tags=[{Key=Name,Value=my-igw}]

step4:Attach Internet Gateway to VPC

aws ec2 attach-internet-gateway --internet-gateway-id igw-0b551b91420950dca --vpc-id vpc-01274c987539e3042

step5:create public and private route table
aws ec2 create-route-table --vpc-id vpc-01274c987539e3042
aws ec2 create-route-table --vpc-id vpc-01274c987539e3042

step6:attach internet gateway to public route table
aws ec2 create-route --route-table-id rtb-02dbeefeac8d0d3b9 --destination-cidr-block 0.0.0.0/0 --gateway-id igw-0b551b91420950dca

step7:create security group for ssh and http
aws ec2 create-security-group --group-name my-ssh --description "My security group" --vpc-id vpc-01274c987539e3042
aws ec2 authorize-security-group-ingress --group-id sg-066892b8f29c454e1 --protocol tcp --port 80 --cidr 10.0.0.0/24

step8: create a ec2 in public subnet

aws ec2 run-instances --image-id ami-063d43db0594b521b --instance-type t2.micro --key-name DevOps --security-group-ids sg-0d7db45cb5672ebd3 sg-066892b8f29c454e1 --subnet-id subnet-05227a1c3b938ea07


step9: create a ec2 in private subnet
aws ec2 run-instances --image-id ami-063d43db0594b521b --instance-type t2.micro --key-name DevOps --security-group-ids sg-0d7db45cb5672ebd3 sg-066892b8f29c454e1 --subnet-id subnet-0e26c3dad277e37c3


