# Freqtrade EC2 Instructions
1. [Create an account on AWS](https://portal.aws.amazon.com/billing/signup#/start) (You get a 1 year free trial)

2. Search for EC2

![search-ec2](./images/search-ec2.png)

3. Click Launch Instance (Launches an EC2 VM)

4. Choose Ubuntu20

5. Select t2.micro (It’s the one that works with a free trial). Click Review and Launch

6. Click Launch

7. Create a keypair (or use your own)

8. Search for EC2 again

9. View your instances

10. With your instance selected, copy the Public IPv4 DNS (it changes about everyday, so if
you need to ssh into this VM again, you have to login and check the ip again, or you can
[allocate an elastic ip](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html#using-instance-addressing-eips-allocating), and [associate it to the VM](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html#using-instance-addressing-eips-associating)

11. Move your ssh key to ~/.ssh and change it’s permissions to 400. (Mac and Linux Operating Systems).
Now you’re able to login to the VM you just created

```
mv ~/Downloads/freqbot.pem ~/.ssh
chmod 400 ~/.ssh/freqbot.pem
ssh -i ~/.ssh/freqbot.pem ubuntu@your_ip.compute.amazonaws.com
```

12. Your ec2 is all good now. You can run this to get freqtrade downloaded and configured,
```
sudo apt-get update
sudo apt-get install python3.9
git clone https://github.com/freqtrade/freqtrade
cd ./freqtrade
./setup.sh -i
```
