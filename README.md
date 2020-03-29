# ec2-server

This is a Terraform code written to spin-up an ec2 instance. Below code will create two security groups named "http and ssh" and will use a keypair already exists in the account.


```
provider "aws" {
    access_key = "[put_your_access_key_here]"
    secret_key = "[put_your_secret_key_here]"
    region = "us-east-2"
}

################################################################
# Creating Security Group for http
################################################################

resource "aws_security_group" "http" {

    name         = "http"
    description  = "allows 80 from all"

    ingress {
        cidr_blocks = ["0.0.0.0/0"]  
        from_port   = 80
        to_port     = 80
        protocol    = "tcp"
      }

     egress {
        from_port       = 0
        to_port         = 0
        protocol        = "-1"
        cidr_blocks     = ["0.0.0.0/0"]
    }

    tags = {
        Name = "http"
    }
}    

################################################################
# Creating Security Group for ssh
################################################################

resource "aws_security_group" "ssh" {

    name         = "ssh"
    description  = "allows 22 from all"

    ingress {
        cidr_blocks = ["0.0.0.0/0"]  
        from_port   = 22
        to_port     = 22
        protocol    = "tcp"
      }

     egress {
        from_port       = 0
        to_port         = 0
        protocol        = "-1"
        cidr_blocks     = ["0.0.0.0/0"]
    }

    tags = {
        Name = "ssh"
    }
}    

################################################################
# Creating EC2 Instance
################################################################

resource "aws_instance" "ec2server" {

    ami = "ami-0e01ce4ee18447327"
    instance_type = "t2.micro"
    key_name = "xxxx-provide-your-key-name-xxxx"
    availability_zone  = "us-east-2a"
    associate_public_ip_address = true
    vpc_security_group_ids = ["${aws_security_group.http.id}",
                               "${aws_security_group.ssh.id}"]
    tags = {
        Name = "ec2server"
  }
}

output "ec2server-instance" {
  value = aws_instance.ec2server
}

```

* Initialize Terraform
```
$terraform init
```
* Check for changes that will be applied up on running the code.
```
$terraform plan
```
* Execute the code as follows
```
$terraform apply
```
* In case you want to remove all the applied changes.
```
$terraform destroy
```
