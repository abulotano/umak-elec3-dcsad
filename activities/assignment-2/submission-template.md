# Assignment 2 Submission

## About me

- GitHub username: abulotano
- Section: IV - DCSAD
- IAM user name that I signed in with: dcsad-g03
- X: 192

---

## Part A. Explore

### A1. The VPC

Default VPC IPv4 CIDR:

`172.31.0.0/16`

Number of addresses in that CIDR:

65,536

### A2. The subnets

| Availability Zone | IPv4 CIDR |
| --- | --- |
| `ap-southeast-1a` | `172.31.0.0/20` |
| `ap-southeast-1b` | `172.31.16.0/20` |
| `ap-southeast-1c` | `172.31.32.0/20` |

![Subnets](<img width="1283" height="237" alt="screenshot-1-subnets" src="https://github.com/user-attachments/assets/ac05c1b2-4002-467e-b2d8-7f69b8cac592" />
)

### A3. Available addresses

Available IPv4 addresses in each subnet:

`ap-southeast-1a` 4,090, `ap-southeast-1b` 4,091, `ap-southeast-1c` 4,091.

Why is the number lower than 4,096?

A `/20` subnet has 4,096 total IP addresses. AWS automatically reserves 5 IP addresses in every subnet for internal networking purposes (network address, VPC router, DNS server, future use, and broadcast address). Therefore, an unused subnet shows 4,096 − 5 = 4,091 available addresses.

What uses the missing address in the subnet with the lowest number?

An active network resource running inside that subnet holds the missing IP address, such as an active EC2 instance, Elastic Network Interface (ENI), Elastic IP, or AWS service endpoint.

### A4. The route table

| Destination | Target |
| --- | --- |
| `172.31.0.0/16` | `local` |
| `0.0.0.0/0` | `igw-0943e7e6f88293168` |

![Route Table]()

### A5. Public or private

Are the default subnets public or private? Which route proves it?

Public. The route table contains a route with destination `0.0.0.0/0` pointing directly to an Internet Gateway (`igw-0943e7e6f88293168`).

### A6. The internet gateway

State of the internet gateway:

Attached

What happens to the default subnets if the gateway is detached?

The route `0.0.0.0/0` loses its target. The subnets lose their connection to the public internet, meaning instances can no longer receive external internet traffic nor initiate outbound connections. Instances can still reach each other locally via the `local` route.

### A7. NAT gateways

Number of NAT gateways:

0

Can a server in a new private subnet download updates? Why?

No. A private subnet does not have a direct route to an Internet Gateway. To download updates, the private subnet needs a route directing `0.0.0.0/0` traffic to a NAT Gateway located in a public subnet.

### A8. The network ACL

| Rule number | Source | Allow or Deny |
| --- | --- | --- |
| 100 | `0.0.0.0/0` | Allow |
| `*` | `0.0.0.0/0` | Deny |

How is a network ACL different from a security group?

A network ACL operates at the subnet level, whereas a security group operates at the individual instance level. Network ACLs are stateless (requiring separate rules for inbound and outbound traffic) and support both Allow and Deny rules. Security groups are stateful (allowed inbound traffic automatically permits outbound reply traffic) and only support Allow rules.

![Network ACL](<img width="1281" height="205" alt="screenshot-3-network-acl" src="https://github.com/user-attachments/assets/98a209b1-3147-4b1e-9917-3851462c936a" />
)

### A9. The default security group

Inbound rule (type and source):

All traffic, from `sgr-0b795189f3efb993c`. The source is the `default` security group itself.

Which resources can send traffic to an instance that uses it?

Only resources assigned to the exact same `default` security group. Traffic from any other source is blocked.

---

## Part B. Prepare

### B1. Plan two subnets

- Public subnet CIDR: `10.192.1.0/24`
- Private subnet CIDR: `10.192.2.0/24`

### B2. Route tables

Route table of the public subnet:

| Destination | Target |
| --- | --- |
| `10.192.0.0/16` | `local` |
| `0.0.0.0/0` | internet gateway |

Route table of the private subnet:

| Destination | Target |
| --- | --- |
| `10.192.0.0/16` | `local` |

### B3. My VPC diagram

Tool used (Excalidraw, draw.io, Lucidchart, or paper):

Excalidraw

![B3: my VPC diagram](vpc-diagram.png)

### B4. Predict a change

Can you still open the web page from your laptop? Why?

No. Deleting the `0.0.0.0/0` route to the Internet Gateway breaks network routing between the public internet and the subnet. Without a route to the IGW, traffic cannot reach or return to your laptop.

Can the instance still reach another instance in the VPC? Why?

Yes. Intra-VPC communication uses the `10.192.0.0/16` -> `local` route, which remains active in the route table.

### B5. Place a database

Which subnet gets the database? Why?

The private subnet, `10.192.2.0/24`. Keeping the database in a private subnet prevents direct internet exposure and protects sensitive data from external attacks, while allowing web servers in the public subnet to reach it via the local VPC route.

### B6. My question about VPCs

What is your question, and what made you think of it?

If two separate VPCs are created with overlapping CIDRs (such as two VPCs both using `10.192.0.0/16`), can they still be connected using VPC Peering, or must one VPC be recreated with a different address range? I thought of this because many students in our section use similar CIDR ranges for their assignments.
