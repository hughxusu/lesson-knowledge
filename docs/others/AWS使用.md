# AWS使用

## 环境搭建

通过New VPC Experience按钮切换控制面板模式

### 创建VPC

1. 创建弹性ip，弹性ip->分配新地址。
2. vpc控制面板launch vpc wizard。
3. 带公有子网和私有子网的vpc，并创建两个子网，两个子网不能在一个可用区。（数据库子网组需要两个子网）

| Name      | VPC     | IPv4 CIDR   | 可用区          |
| :-------- | :------ | :---------- | --------------- |
| 公有子网  | battle4 | 10.0.0.0/24 | ap-northeast-2c |
| 私有子网  | battle4 | 10.0.1.0/24 | ap-northeast-2c |
| 私有子网2 | battle4 | 10.0.2.0/24 | ap-northeast-2a |

4. 填写vpc名称并选择分配的弹性ip。

5. 编辑路由表

|                | Destination | Target                                                       | Status | Propagated |
| -------------- | :---------- | :----------------------------------------------------------- | :----- | :--------- |
| 共有子网路由表 | 10.0.0.0/16 | local                                                        | active | No         |
|                | 0.0.0.0/0   | [igw-0f3cf782dbfdd52b0](https://ap-northeast-2.console.aws.amazon.com/vpc/home?region=ap-northeast-2#igws:internetGatewayId=igw-0f3cf782dbfdd52b0) | active | No         |
| 私有子网路由表 | 10.0.0.0/16 | local                                                        | active | No         |
|                | 0.0.0.0/0   | [nat-0558c7db8dc25990f](https://ap-northeast-2.console.aws.amazon.com/vpc/home?region=ap-northeast-2#NatGateways:natGatewayId=nat-0558c7db8dc25990f) | active | No         |

6. 创建安全组

|          | 类型     | 协议 | 端口范围 | 源          | 描述 -可选 |
| -------- | -------- | ---- | -------- | ----------- | ---------- |
| 私有子网 | 所有流量 | 全部 | 全部     | 10.0.0.0/16 | private    |
| 共有子网 | SHH      | TCP  | 22       | 0.0.0.0/0   | public     |

7. 创建vpc。

###  创建数据库

1. 创建子网组，选择vpc并加入所有子网，至少有两个子网，且不在一个可用区。
2. 创建数据库
   * 填写用户名密码
   * 加入vpc中，并选择子网组。
   * 公开访问为否，安全组选择私有。

### 创建ec2

1. 创建实例，使用Amazon Linux 2 AMI是基于centos系统构建。
2. 选择子网，为私有子网，选择私有安全组。
3. 创建堡垒机，选择共有子网，自动分配共有ip打开。共有和私有的安全组全部添加。
4. 登录ec2，用户名ec2-user，私有安全秘钥登录。
5. 安装mysql `sudo yum install mysql`

#### ec2安装docker

```shell
sudo yum update -y
sudo amazon-linux-extras install docker # Amazon Linux 2
sudo service docker start
sudo usermod -a -G docker ec2-user # ec2-user添加到docker组
docker info # 验证运行
```



