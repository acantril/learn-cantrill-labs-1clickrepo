sudo dnf install amazon-cloudwatch-agent


# IAM ROLE
Create an IAM role
Type : EC2 Role
Add Managed Policy `CloudWatchAgentServerPolicy`
And `AmazonSSMFullAccess`
Call the role `CloudWatchRole`

Attach the role to the EC2 instance created by the 1-click deployment.

sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
# Accept all defaults, until default metrics .. pick advanced.

# then when asking for log files to monitor

# 1 /VAR/LOG/SECURE
/var/log/secure
/var/log/secure
(Accept default instance ID)
Accept the default retention option

# 2 /var/log/httpd/access_log
/var/log/httpd/access_log
/var/log/httpd/access_log
(Accept default instance ID)
Accept the default retention option

# 3 /var/log/httpd/error_log
/var/log/httpd/error_log
/var/log/httpd/error_log
(Accept default instance ID)
Accept the default retention option

Answer no to any more logs
Save config into SSM
Use the default name.

# Config will be stored in /opt/aws/amazon-cloudwatch-agent/bin/config.json and stored in SSM

# Bug Fix (these are needed else the agent won't start)
sudo mkdir -p /usr/share/collectd/
sudo touch /usr/share/collectd/types.db

# Load Config and start agent
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c ssm:AmazonCloudWatch-linux -s


# CLEANUP

Delete the CFN stack
You can leave the SSM parameter store value
Delete the CwLog Groups for /var/log/secure, /var/log/httpd/access_log & /var/log/httpd/error_log
Delete the CloudWatchRole instance role you created.