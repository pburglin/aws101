# Description
This course was designed to help learners get started with AWS. It includes detailed steps to setup their free tier AWS account, sample apps and have them deployed in AWS.

I will include notes for Windows users, but these steps were developed and tested on a Mac. You should be able to translate these commands to Windows. For simplicity, I will use Atom as my editor of choice.

# Pre-reqs

1. Create a free AWS Account
   * Go to the [AWS page](https://aws.amazon.com)
   * Get familiar with the details under "AWS Free Tier Details"
   * Click the "Create a Free Account" button and follow the forms to create your account

NOTE: Concerned with unexpected bills for usage above the free tier? If you are careful this is typically not a problem, but you can always consider using a reloadable prepaid card instead of an actual credit card.

2. AWS CLI
   * Follow the instructions [here](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) to install the AWS CLI and to include it in your command line path.

3. Java Development Kit 8

4. Git
   * Check if your system already has a Git client installed; If not, follow the instructions [here](https://git-scm.com/downloads) to install it.

NOTE: Is this your first time using Git? Set it up with your information:

```
git config --global user.name "Your Name Here"
git config --global user.email youremail@example.com
```

5. A good text editor, such as Atom, Visual Studio Code, Notepad++ etc 

6. A SSH client and your very own unique keys

   * Macs:
Macs have Terminal installed OOB; Many developers prefer iTerm2:
```
# install homebrew
ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"
brew cask install iterm2
```

   * Windows:
Windows users should download and install **Putty** SSH client from [here](http://www.putty.org). The main package comes bundled with **PuTTYGen** (use this to generate your keys) and **pscp** (use this to copy files over SSH).

Create your SSH keys:

Mac / Linux:
```
ssh-keygen -t rsa

Your identification has been saved in /Users/myname/.ssh/id_rsa.
Your public key has been saved in /Users/myname/.ssh/id_rsa.pub.
The key fingerprint is:
ae:89:72:0b:85:da:5a:f4:7c:1f:c2:43:fd:c6:44:38 myname@mymac.local
The key's randomart image is:
+--[ RSA 2048]----+
|                 |
|         .       |
|        E .      |
|   .   . o       |
|  o . . S .      |
| + + o . +       |
|. + o = o +      |
| o...o * o       |
|.  oo.o .        |
+-----------------+
```

Windows: use PuTTYGen to generate your keys

More info on setting up your keys: https://help.github.com/articles/connecting-to-github-with-ssh/ and https://docs.joyent.com/public-cloud/getting-started/ssh-keys/generating-an-ssh-key-manually/manually-generating-your-ssh-key-in-mac-os-x

7. [Node](https://nodejs.org/en/) - the latest LTS is recommended here

NOTE: On Macs you can also install Node with Brew:

```
brew install node
```

8. Yarn and JHipster:

NOTE: On Macs you can install Yarn and JHipster with Brew:
```
brew install yarn
yarn global add yo
yarn global add generator-jhipster

# make sure Yarn (and JHipster) gets included in your PATH:
export PATH="$PATH:`yarn global bin`:$HOME/.config/yarn/global/node_modules/.bin"

# get rid of git connectivity warnings when using JHipster
git config --global url."https://".inteadOf git://
```

8. [SourceTree](https://www.sourcetreeapp.com) (optional):
SourceTree is a graphical interface for Git. It is especialy helpful to visualize changes in filesets and repositories with multiple branches.

NOTE: SourceTree is free. After its trial period, you can download a free license file here: my.atlassian.com

## Validating pre-reqs:
```
# ~~dependency checks:~~

aws --version
aws-cli/1.11.97 Python/2.7.14 Darwin/17.2.0 botocore/1.5.60

java -version
java version "1.8.0_45"

git --version
git version 2.13.6 (Apple Git-96)

node --version
v8.9.1

npm -version
5.5.1

yarn --version
0.18.2

yo --version
1.8.5

jhipster --version
4.10.2
```

---

# [TodoMVC](http://todomvc.com)
What is it? A frontend-only ToDo app implemented with in most of the popular JavaScript frameworks of today.

Let's deploy our own TodoMVC with S3 - look, no servers!

```
# create local copy of TodoMVC project
cd /tmp
git clone https://github.com/tastejs/todomvc.git

# download all dependencies
cd todomvc/examples/angular2
npm install
```

Go to the [AWS console](https://console.aws.amazon.com) page
In the top menu, click Services / S3
Click "Create bucket", enter a unique DNS name (e.g. pburglin20171112) and click Create
Open the new bucket
Click Properties / Static website hosting select "Use this bucket to host a website"

Click Permissions / Bucket policy, and enter the policy below (change the "pburglin20171112" to your bucket name):
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Action": "s3:GetObject",
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::pburglin20171112/*",
      "Principal": "*"
    }
  ]
}
```

Now let's copy the static files over to our S3 bucket:
```
cd /tmp/todomvc
aws s3 cp . s3://pburglin20171112/ --recursive
```

Once this is finished we should now be able to access static files hosted directly from our S3 bucket:

https://s3.amazonaws.com/pburglin20171112/examples/angular2/index.html

NOTE: hosting files directly from S3 buckets is fine for training. However, for real apps you should take a look at AWS CloudFront to serve static files, as it will be safer and give you better performance.

---

# AWS dashboard and EC2 instances

Go to the [AWS console](https://console.aws.amazon.com) page

In the top menu, click Services / EC2
In the top right, confirm you have selected the "N. Virginia" region
Click "Launch Instance"
Pick the latest Ubuntu Server 16.04 Linux AMI
Click the "Next" buttons, and review each screen - do not change anything
Under "Configure Security Group": keep the default rule for SSH and add new rules for HTTP and HTTPS
If this is your first time creating EC2 instances, you will need to setup keys with AWS - just upload the public key (~/.ssh/id_rsa.pub) you created earlier.

As the EC2 instance starts up, in the EC2 Instances screen, with the new instance selected, take note of these 2 fields:
* Public DNS (IPv4) - on my case, it was ec2-34-227-26-150.compute-1.amazonaws.com
* IPv4 Public IP - on my case, it was 34.227.26.150

NOTE: Your public DNS and IP will be different than the ones I got here. So for the next steps make sure to replace these values in the instructions below with the ones you got from AWS.

Let's try connecting to our new EC2 instance:
```
ssh -i ~/.ssh/id_rsa ubuntu@34.227.26.150

Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.4.0-1038-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

ubuntu@ip-172-31-76-103:~$
```

NOTE: It might take couple minutes for the EC2 instance to startup completely.

Let's install some dependencies in our EC2 instance:
```
sudo apt-get update
sudo apt-get install -y git
sudo apt-get install -y nodejs
sudo apt-get install -y npm
sudo apt-get install -y nginx
sudo apt-get install -y openjdk-8-jdk
```

NOTE: If everything worked properly, we should have a Ubuntu server with Java and other dependencies ready to run our apps.

---

# [JHipster](http://www.jhipster.tech)
What is it? A development platform to generate, develop and deploy Spring Boot + Angular Web applications and Spring microservices.

Let's build our sample BookMart app locally, and review key aspects of the app:

### Let's create our local Git repo and Bookmart app

```
mkdir -p /tmp/bookmart
cd /tmp/bookmart

# Create local Git repository with initial project
git init
git add .
git status

# nothing to commit (create/copy files and use "git add" to track)
```

```
jhipster
```

Review each question presented by JHipster, follow the prompts with default values and override only these options:
1. Enable **Search engine using Elasticsearch**;
2. Pick **English** as the app's main language, **Spanish** as an additional language;

### Let's review what we have now:
```
./mvnw clean test
...
[INFO] Analyzed bundle 'Bookmart' with 70 classes
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 01:04 min
[INFO] Finished at: 2017-11-12T12:46:11-05:00
[INFO] Final Memory: 54M/384M
[INFO] ------------------------------------------------------------------------
```

```
./mvnw
...
----------------------------------------------------------
	Application 'bookmart' is running! Access URLs:
	Local: 		http://localhost:8080
	External: 	http://192.168.86.218:8080
	Profile(s): 	[swagger, dev]
----------------------------------------------------------
```

[Local Bookmart - Without Hot Reload](http://localhost:8080)

* Sample user accounts:
  * admin/admin
  * user/user

### Hot reload
```
# on a 2nd terminal:
cd /tmp/bookmart
yarn start
```

[Local Bookmart - With Hot Reload](http://localhost:9000)

```
cd /tmp/bookmart
atom .
```
Let's make few changes to src/main/webapp/app/home/home.component.html and check out the browser tab

### Create the Author data entity:

```
jhipster entity author
```

Create this new entity with the following fields:
* author:
  * Field name: name
    * Type: String
    * Validation: Yes
      * Check Required
  * No entity relationship
  * DTO: No DTO, use the entity directly
  * Service class: No separate service class
  * Pagination: Yes, with infinite scroll

#### Commit code changes to local Git repo
```
// review updates in SourceTree
git add .
git status
git commit -m "Added Author entity"
```

### Review code changes with SourceTree

### Create the Book data entity:

```
jhipster entity book
```

Create the entity with the following fields:
* book:
  * title
    * Type: String
    * Validation: Yes
      * Check Required
  * price
    * Type: BigDecimal
    * Validation: Yes
      * Check Required
  * Yes to Add a relationship to another entity:
    * Other entity = author
    * Relationship name = author
    * Relationship type = many to one	<-- simplified use case, books can only have 1 author
    * When displaying the relationship, display = name
    * Relationship validation = Yes, Required    
  * DTO: No DTO, use the entity directly
  * Service class: No separate service class
  * Pagination: Yes, with infinite scroll

#### Commit code changes to local Git repo
```
// review updates in SourceTree
git add .
git status
git commit -m "Added Book entity"
```

### Now let's deploy it in our AWS EC2 instance:

There are many ways to have our app deployed in AWS. For this part we are 


Here we will explore two ways to deploy our app:

#### From the EC2 instance, checkout from Git repo, build and run


From your workstation, do a local build and upload binary package to EC2 instance

```
# build the application binary:
./mvnw package

ls -la target/*.war
-rwxr--r--  1 pedroburglin  wheel  78564956 Nov 12 14:30 target/bookmart-0.0.1-SNAPSHOT.war
```

```
# upload and start the app from our EC2 instance:
scp -i ~/.ssh/id_rsa target/bookmart-0.0.1-SNAPSHOT.war ubuntu@34.227.26.150:~/

ssh -i ~/.ssh/id_rsa ubuntu@34.227.26.150

java -jar bookmart-0.0.1-SNAPSHOT.war
...
----------------------------------------------------------
	Application 'bookmart' is running! Access URLs:
	Local: 		http://localhost:8080
	External: 	http://172.31.76.103:8080
	Profile(s): 	[swagger, dev]
----------------------------------------------------------
```

Now let's try our Bookmart app running from AWS: in your browser go to http://34.227.26.150:8080/ or http://ec2-34-227-26-150.compute-1.amazonaws.com:8080/

If everything worked properly you should see Bookmart's login page!

NOTE: If you want the app to continue running after you close the SSH terminal, you can start it with this command below:
```
nohup java -jar bookmart-0.0.1-SNAPSHOT.war > nohup.out &
```

---

# [AWS CodeStar](https://aws.amazon.com/codestar/)
What is it? A unified user interface to quick-start and manage your software development activities in a single place.



---

# Want to learn more ?

[A Cloud Guru AWS Certified Solutions Architect](https://acloud.guru/learn/aws-certified-solutions-architect-associate)
   * Great AWS certification exam preparation materials

[Cloud Academy AWS Certified Solutions Architect](https://cloudacademy.com/learning-paths/solutions-architect-associate-certification-preparation-for-aws-14/)
   * Great guided hands on labs, safely access AWS, Azure and Google through Cloud Academy without risk of additional charges
