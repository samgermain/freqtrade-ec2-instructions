# Freqtrade EC2 Instructions

### 1. [Create an account on AWS](https://portal.aws.amazon.com/billing/signup#/start) (You get a 1 year free trial)

### 2. Search for EC2

![search-ec2](./images/search-ec2.jpg)

### 3. Click Launch Instance
- Launches an EC2 VM (Virtual Machine)

![launch instance](./images/launch-instance.jpg)

### 4. Choose Ubuntu20

![](./images/select-ubuntu.jpg)

### 5. Select t2.micro

- It’s the one that works with a free trial 

### Click Review and Launch

##### Note

- You may have trouble installing ta-lib from the `setup.sh` script on a t2-micro, sometimes it works, sometimes it doesn't. You can try to non free-trail t2.small for better results.

![](./images/t2-micro.jpg)

### 6. Click Launch

![](./images/launch.jpg)

### 7. Create a keypair (or use your own)

![](./images/download-key-pair.jpg)
![](./images/launch-instances.jpg)

### 8. Search for EC2 again

![](./images/search-ec2.jpg)

### 9. View your instances

![](./images/select-instances.jpg)

### 10. With your instance selected, copy the Public IPv4 DNS
- it changes about everyday, so if you need to ssh into this VM again, you have to login and check the ip again, or you can
[allocate an elastic ip](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html#using-instance-addressing-eips-allocating), and [associate it to the VM](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html#using-instance-addressing-eips-associating)

![](./images/ipv4.jpg)

### 11. Move your ssh key to ~/.ssh and change it’s permissions to 400. (Mac and Linux Operating Systems)
Now you’re able to login to the VM you just created

```
mv ~/Downloads/freqbot.pem ~/.ssh
chmod 400 ~/.ssh/freqbot.pem
ssh -i ~/.ssh/freqbot.pem ubuntu@your_ip.compute.amazonaws.com
```

![](./images/ssh.jpg)

### 12. Your ec2 is all good now. You can run this to get freqtrade downloaded and configured
```
sudo apt-get update
sudo apt-get install python3.9
git clone https://github.com/freqtrade/freqtrade
cd ./freqtrade
./setup.sh -i
```

##### NOTE 
If your installation hangs on `Running setup.py install for TA-Lib ../` you can try
- Using a t2.small(not free-trial eligible) instead of a t2 micro
- using `clang` as a C compiler instead of `gcc` (If you figure out how to use clang, a PR with instructions would be appreciated)
- Deactivating your ec2 and setting up a new one
- Exiting your session and trying to install a different day (sometimes it just doesn't work)

### 13. Screen is a command that you can use to run programs as a background process
 
When you run a program using screen on virtual machine that you have connected to remotely (e.g. via ssh), you can close your connection, and the program will keep running. Run these commands to execute freqtrade using screen

```
sudo apt-get install screen
screen -dm freqtrade -s YourStrategy
```

Freqtrade is now running in the background and you can `exit` out of your connection, close your terminal, turn off your computer, and freqtrade will still be running. 

If you want to check the console output of your execution of freqtrade, you can reconnect to your VM and run `screen -r` to open your execution in the foreground. Once you are done checking it, press ctrl+d followed by ctrl+a
