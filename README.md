# Halo Development Environment Setup

This guide details the setup of the Halo development environment, including component-specific configurations.

## Domain Registration
- Domain has been purchased by client himself on Route53

## SSL Certificates
1. Currently, we've obtained SSL certificates from AWS Certificate Manager for the following domains:
   - *.bookingbyhalo.com
   - dev.api.bookingbyhalo.com
2. The SSL certificates are employed with AWS services as follows:
   - Application Load Balancer
   - CloudFront

## Custom VPC
1. Created a Custom VPC with the following specifications:
    - **VPC CIDR**: `172.10.0.0/16`
    - **Public Subnets**: `172.10.0.0/24`, `172.10.16.0/24`
    - **Private Subnets**: `172.10.128.0/24`, `172.10.144.0/24`
    - **Datanet Subnets**: `172.10.160.0/24`, `172.10.176.0/24`
    - **Route Tables**: Public, Private, Datanet
    - **Internet Gateway**: 1
    - **NAT Gateway**: 0
    - **Security Groups**: 3

## Setup oidc for github to access the aws resources with its associated IAM role and permission
1. Setup oidc on aws for github:
    - **Configure** OIDC for AWS Identity Provider
    - **Goto**  Identity provider in IAM
    - **Add** an Identity provider
    - **Select type**: OpenID Connect
    - **Provider URL**: https://token.actions.githubusercontent.com
    - **Audience**: sts.amazonaws.com
    - **Generate** thumbprint
    - **After** the identity creation click on Assign Role
    - **Select Role option**: Create a new Role
    - **Select Trusted entity type**: Custom trust policy
    - **Trust policy example**: `backend/new_docs/trust-entity-for-github-to-aws-oidc-role.json`
    - **Then** add Permissions policy as customer managed/ aws managed
    - **Customer managed permission example**: `backend/new_docs/github-to-aws-oidc-policy.json`
    - **Give**: a descriptive name to the role and click create role
    - **Click**: Create Role
**This role will be assumed by github action pipeline later for frontend and backend deployments**

## Creating an ALB
1. Creating an Application Load Balancer (ALB) with 80 and 443 Listeners:
    - Go to the EC2 Dashboard by clicking on "Services" in the top menu and selecting "EC2" under "Compute".
    - In the EC2 Dashboard, under the "Load Balancing" section in the navigation pane, click on "Load Balancers".
    - Click on the "Create Load Balancer" button.
    - Select "Application Load Balancer" as the load balancer type.
    - Configure Load Balancer
        - **Name**: Enter a name for your load balancer.
        - **Scheme**: Choose the appropriate scheme (usually "internet-facing").
        - **IP address type**: Select "ipv4".
        - **Listeners**:
    - Add two listeners:
        - For the first listener, select "HTTP" (port 80).
        - For the second listener, select "HTTPS" (port 443).
    - Configure Security Settings (Optional)
        - If you're setting up HTTPS, you can configure SSL certificates and security policies in this step.
    - Configure Routing
    - Configure target groups and routing rules based on your application's requirements.
    - Register Targets
        - Register EC2 instances or other targets to your target groups.
    - Review and Create
        - Review your load balancer configuration and click on "Create" to create the ALB.
    - Access Your ALB
        - Once the ALB is created, you can access it using the provided DNS name.

## Database Setup
    Navigate to the `database/README.md` for database setup instructions.

## Frontend Setup
    Navigate to the `frontend/README.md` for frontend setup instructions.

## Backend Setup
    Navigate to the `backend/README.md` for backend setup instructions.

## Network/ Architecture diagram
    Navigate to the `diagram/` for network/architecture diagram.
