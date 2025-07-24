# README.md
# Redis Caching Setup on EC2 with RedisInsight

## Step 1: Launch and Prepare Your EC2 Instance

1. Login to AWS Console.
2. Navigate to **EC2 > Instances > Launch Instance**.
3. Choose the following:
   - **AMI:** Amazon Linux 2 (or Ubuntu if preferred)
   - **Instance type:** t2.micro (free tier eligible)
   - Configure storage as needed.
   - **Security Group Inbound Rules:**
     - SSH (Port 22) – Your IP
     - Redis (Port 6379) – Your IP or `0.0.0.0/0` (for testing only)
     - RedisInsight (Port 8001) – Your IP or `0.0.0.0/0`
4. Launch the instance and connect using SSH.

---

## Step 2: Install Redis on the EC2 Instance

### For Amazon Linux 2:
```bash
sudo yum update -y
sudo amazon-linux-extras enable redis6
sudo yum install redis -y

#**For Ubuntu:**
sudo apt update
sudo apt install redis-server -y

Start and enable Redis:
sudo systemctl enable redis-server
sudo systemctl start redis-server

Verify Redis is running:

redis-cli ping
# Expected output: PONG

Step 3: Configure Redis for Remote Access
1.Edit Redis config:

sudo nano /etc/redis/redis.conf

2.Modify:

Comment out or remove:
bind 127.0.0.1
Set:
protected-mode no

3.Save and exit (Ctrl+O, Enter, Ctrl+X).

4.Restart Redis:

sudo systemctl restart redis

Step 4: Install RedisInsight on EC2
Download RedisInsight:

wget https://downloads.redisinsight.redis.com/latest/RedisInsight-linux64 -O redisinsight

chmod +x redisinsight

Run RedisInsight:
./redisinsight &


You should see:
Active: active (running)


Step 5: Access RedisInsight Web UI
1. Open browser:
http://<EC2_PUBLIC_IP>:8001

2.Add a new Redis database:

Host: EC2 IP

Port: 6379

3.Connect and monitor.


Step 6: Monitor Redis
Using RedisInsight you can:

View keys and values.

Monitor memory and performance.

Analyze command history.

Explore Redis modules.

