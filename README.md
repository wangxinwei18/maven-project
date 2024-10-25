# maven-project
Source code for Sunny's Jenkins course at Udemy.
1. Check scp for windows: Run PowerShell as Administrator:

  Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH.Client*'

if not installed, install it:

Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

2. Sign in EC2 instance and change tomcat9 directory permissions for ec2-user:

$> sudo chown ec2-user /var/lib/tomcat9/webapps/

3.  Run the sample pipeline before, update your parameter in "xxxxxxxxxx"
