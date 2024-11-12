AWSTemplateFormatVersion: 2010-09-09
Description: Build Vpc
Resources:
  Task8VPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 192.168.0.0/16
      EnableDnsSupport: 'true'
      EnableDnsHostnames: 'true'
      Tags:
       - Key: Name
         Value: Task8-Vpc

  Task8InternetGateway:
    Type: AWS::EC2::InternetGateway
    Properties:
      Tags:
      - Key: Name
        Value: Task8-IGW

  Task8AttachGateway:
    Type: AWS::EC2::VPCGatewayAttachment
    Properties:
      VpcId:
         Ref: Task8VPC
      InternetGatewayId:
         Ref: Task8InternetGateway

  Task8PubSubnet:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref Task8VPC
      CidrBlock: 192.168.0.0/24
      AvailabilityZone: "us-east-1a"
      Tags:
      - Key: Name
        Value: Task8-Pub-Sub

  Task8PvtSubnet:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref Task8VPC
      CidrBlock: 192.168.1.0/24
      AvailabilityZone: "us-east-1a"
      Tags:
      - Key: Name
        Value: Task8-Pvt-Sub

  Task8PubRouteTable:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId:  
        Ref: Task8VPC
      Tags:
      - Key: Name
        Value: Task8-Pub-Rt

  Task8PvtRouteTable:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId:  
        Ref: Task8VPC
      Tags:
      - Key: Name
        Value: Task8-Pvt-Rt

  Task8PubSubnetRouteTableAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId:
        Ref: Task8PubSubnet
      RouteTableId:
        Ref: Task8PubRouteTable

  Task8PvtSubnetRouteTableAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId:
        Ref: Task8PvtSubnet
      RouteTableId:
        Ref: Task8PvtRouteTable

  Task8Route:
    Type: AWS::EC2::Route
    DependsOn: Task8AttachGateway
    Properties:
       RouteTableId:
         Ref: Task8PubRouteTable
       DestinationCidrBlock: 0.0.0.0/0
       GatewayId:
         Ref: Task8InternetGateway
  InstanceSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Allow http to client host
      VpcId: !Ref Task8VPC
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0
      SecurityGroupEgress:
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0

  InstanceSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Allow ssh to client host
      VpcId: !Ref Task8VPC
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 0.0.0.0/0
      SecurityGroupEgress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 0.0.0.0/0

  MyKeyPair:
    Type: AWS::EC2::KeyPair
    Properties:
      KeyName: MyKeyPair

  
  
  
 
