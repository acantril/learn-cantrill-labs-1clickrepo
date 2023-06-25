# INSTANCE A


df -k
sudo mkdir -p /efs/wp-content
sudo dnf -y install amazon-efs-utils
cd /etc
sudo nano /etc/fstab

file-system-id:/ /efs/wp-content efs _netdev,tls,iam 0 0 

sudo mount /efs/wp-content
df -k
cd /efs/wp-content
sudo touch amazingtestfile.txt

# INSTANCE B

df -k
sudo dnf -y install amazon-efs-utils
sudo mkdir -p /efs/wp-content
sudo nano /etc/fstab
file-system-id:/ /efs/wp-content efs _netdev,tls,iam 0 0
sudo mount /efs/wp-content
ls -la