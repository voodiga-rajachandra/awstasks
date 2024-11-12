AWSTemplateFormatVersion: 2010-09-09
Description: Build Vpc
Resources:
  AjaVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 192.168.0.0/16
      EnableDnsSupport: 'true'
      EnableDnsHostnames: 'true'
      Tags:
       - Key: Name
         Value: Aja-Vpc

  AjaInternetGateway:
    Type: AWS::EC2::InternetGateway
    Properties:
      Tags:
      - Key: Name
        Value: Aja-IGW

  AjaAttachGateway:
    Type: AWS::EC2::VPCGatewayAttachment
    Properties:
      VpcId:
         Ref: AjaVPC
      InternetGatewayId:
         Ref: AjaInternetGateway

  AjaPubSubnet:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref AjaVPC
      CidrBlock: 192.168.0.0/24
      AvailabilityZone: "us-east-1a"
      Tags:
      - Key: Name
        Value: Aja-Pub-Sub

  AjaPvtSubnet:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref AjaVPC
      CidrBlock: 192.168.1.0/24
      AvailabilityZone: "us-east-1b"
      Tags:
      - Key: Name
        Value: Aja-Pvt-Sub

  AjaPubRouteTable:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId:  
        Ref: AjaVPC
      Tags:
      - Key: Name
        Value: Aja-Pub-Rt

  AjaPvtRouteTable:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId:  
        Ref: AjaVPC
      Tags:
      - Key: Name
        Value: Aja-Pvt-Rt

  AjaPubSubnetRouteTableAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId:
        Ref: AjaPubSubnet
      RouteTableId:
        Ref: AjaPubRouteTable

  AjaPvtSubnetRouteTableAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId:
        Ref: AjaPvtSubnet
      RouteTableId:
        Ref: AjaPvtRouteTable

  AjaRoute:
    Type: AWS::EC2::Route
    DependsOn: AjaAttachGateway
    Properties:
       RouteTableId:
         Ref: AjaPubRouteTable
       DestinationCidrBlock: 0.0.0.0/0
       GatewayId:
         Ref: AjaInternetGateway

  SSHSecurityGroup:
    Type: "AWS::EC2::SecurityGroup"
    Properties:
      GroupDescription: "Enable SSH access"
      VpcId: !Ref AjaVPC
      SecurityGroupIngress:
        - IpProtocol: "tcp"
          FromPort: 22
          ToPort: 22
          CidrIp: "0.0.0.0/0"
      Tags:
        - Key: Name
          Value: "SSHSecurityGroup"
 
  HTTPSecurityGroup:
    Type: "AWS::EC2::SecurityGroup"
    Properties:
      GroupDescription: "Enable HTTP access"
      VpcId: !Ref AjaVPC
      SecurityGroupIngress:
        - IpProtocol: "tcp"
          FromPort: 80
          ToPort: 80
          CidrIp: "0.0.0.0/0"
      Tags:
        - Key: Name
          Value: "HTTPSecurityGroup"


  

  PublicInstance:
    Type: "AWS::EC2::Instance"
    Properties:
      InstanceType: "t2.micro"
      SubnetId: !Ref AjaPubSubnet
      ImageId: "ami-0984f4b9e98be44bf"  
      SecurityGroupIds:
        - !Ref SSHSecurityGroup
        - !Ref HTTPSecurityGroup
      KeyName: "cfvpc"
      Tags:
        - Key: Name
          Value: "PublicInstance"
 
  PrivateInstance:
    Type: "AWS::EC2::Instance"
    Properties:
      InstanceType: "t2.micro"
      SubnetId: !Ref AjaPvtSubnet
      ImageId: "ami-0984f4b9e98be44bf"
      SecurityGroupIds:
        - !Ref SSHSecurityGroup
        - !Ref HTTPSecurityGroup
      KeyName: "cfvpc"
      Tags:
        - Key: Name
          Value: "PrivateInstance"
 
