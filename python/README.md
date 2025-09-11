# AWS Infrastructure Dashboard 🚀

## Project Overview
This Flask-based web application provides a real-time dashboard for monitoring AWS infrastructure resources. It offers a clean, table-based visualization of various AWS services and resources in your account.

## Features 🌟
- **EC2 Instance Monitoring**
  - Instance ID
  - Current State
  - Instance Type
  - Public IP Address
- **VPC Information**
  - VPC ID
  - CIDR Block ranges
- **Load Balancer Details**
  - Load Balancer Names
  - DNS Names
- **AMI Management**
  - AMI IDs
  - AMI Names (self-owned only)

## Prerequisites 📋
- Python 3.x
- AWS Account with appropriate permissions
- AWS credentials configured
- Required Python packages (see [Dependencies](#dependencies))

## Dependencies 📦
The following Python packages are required:
```
Flask==3.1.2
boto3==1.40.28
botocore==1.40.28
```
For a complete list of dependencies, see `requirements.txt`.

## Installation 💻

1. Clone the repository:
```bash
git clone https://github.com/TomerHanan/Rolling-Project.git
cd Rolling-Project/infra-automation/src
```

2. Install required packages:
```bash
pip install -r requirements.txt
```

3. Set up AWS credentials as environment variables:
```bash
# Windows PowerShell
$env:AWS_ACCESS_KEY_ID="your_access_key"
$env:AWS_SECRET_ACCESS_KEY="your_secret_key"
$env:AWS_REGION="your_preferred_region"
```

## Usage 🚀

1. Start the application:
```bash
python AWS_INFRA_INFO.py
```

2. Access the dashboard:
   - Open your web browser
   - Navigate to `http://localhost:5001`
   - The dashboard will display your AWS resources in tabular format

## Technical Details 🔧

### Application Structure
- **Flask Web Server**
  - Host: 0.0.0.0 (accessible from any IP)
  - Port: 5001
  - Debug mode: Enabled (disable in production)

### AWS Services Integrated
- EC2 (Elastic Compute Cloud)
- VPC (Virtual Private Cloud)
- ELB (Elastic Load Balancing)
- AMI (Amazon Machine Images)

### Security Considerations 🔒
- Store AWS credentials securely
- Never commit credentials to version control
- Consider using AWS IAM roles for production
- Implement proper authentication in production
- Disable debug mode in production

## Customization 🎨
You can modify the HTML template in `AWS_INFRA_INFO.py` to:
- Add more AWS resources
- Customize the table layout
- Add styling
- Include additional resource information

## Troubleshooting 🔍
Common issues and solutions:

1. **Connection Error**
   - Verify AWS credentials are set correctly
   - Check network connectivity
   - Ensure proper IAM permissions

2. **No Data Displayed**
   - Confirm AWS region is set correctly
   - Verify resources exist in the specified region
   - Check IAM permissions for specific services

## Contributing 🤝
1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a new Pull Request

## License 📄
[Add your preferred license]

## Author ✍️
TomerHanan

---
*This project is part of the Rolling-Project repository*