# MarketPeak Ecommerce Website
Ecommerce Platform Deployment with Git, Linux, and AWS

## Steps to deploying the Ecommerce Platform to an AWS Amazon Linux instance in 13 simple steps
### 1. Create a working directory and change directory to it
```
mkdir MarketPeak_Ecommerce
```
### 2. Initialize the local get repo
```
git init
```
### 3. Download and extract website template
```
wget https://www.tooplate.com/zip-templates/2130_waso_strategy.zip
unzip 2130_waso_strategy.zip
```
### 4. Copy template files to the working directory (MarketPeak_Ecommerce)
```
cp -r 2130_waso_strategy/* .
rm -r 2130_waso_strategy
```
### 5. Create a remote repository (e.g. github, gitlab) and add it to the local repo
```
git remote add origin https://<personal access token>@github.com/<username>/MarketPeak_Ecommerce
```
### 6. Configure git credentials
```
git config --global user.name "<your name>"
git config --global user.email "<your email>"
```
### 7. Add template files to git
```
git add .
git commit -m "Initializing repo and adding template files"
git push -u origin main
```
### 8. Create an AWS amazon linux instance
-   Log in to amazon AWS console
    navigate to EC2
    create an instance and select Amazon Linux AMI
    SSH to your instance using your key-pair
### 9. Add instance public key to remote repository (e.g. Github)
```
ssh-keygen      # follow the prompts to generate the public and private key or continually press enter till it's done
cat ~/.ssh/id_rsa.pub       # This display the public key, then copy the key to your remote repository
```
### 10. Clone the remote repository to your instance
```
git clone git@github.com:yourusername/MarketPeak_Ecommerce.git      # using HTTP, git clone https://github.com/yourusername/MarketPeak_Ecommerce.git
```
### 11. Update your system and Install Apache
```
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```
### 12. Configure HTTPD for your website
```
sudo rm -rf /var/www/html/*
sudo cp -r ~/MarketPeak_Ecommerce/* /var/www/html/
sudo systemctl reload httpd
```
### 13. Add rule to Security Group to open port 80 for serving HTTP traffic
- Go to your EC2 dashboard and select your instance
- Select security in the panel below and click the security group attached
- Click edit inbound rule to add an inbound rule
- Add port range of 80 to open port for HTTP traffic and select your source range   Note: opening all port range makes your application susceptible to attacks, therefore, open only ports you need
- Save rule and access your website at your IP address (e.g. 172.162.34.1)


## Challenges and Solutions
### 1. Git branching Issue
#### Issue:
local repository was initialize to use "master" while the remote repository was "main"
#### Solution:
- Created a new branch for main locally     (```git branch main```)
- Rebased the main branch by pulling the remote repository and set Git Configuration for pull.rebase to true    (```git config pull.rebase=true && git pull origin main```)
- Pushed the main branch to the remote repository   (```git push origin main```)
- Deleted the master branch locally and remotely    (```git branch -D master```)

### 2. Permission for updating the httpd project files in /var/www/html
#### Issue:
I wanted to update the project code in the httpd folder but git pull was not permitted in the folder
#### Solution:
Updated the code in the home directory and copy to the httpd folder
