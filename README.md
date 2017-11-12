# Description
This course was designed to help learners get started with AWS. It includes detailed steps to setup their free tier AWS account, sample apps and have them deployed in AWS.

# Pre-reqs

1. Create a free AWS Account
   * Go to the [AWS console](https://aws.amazon.com) page
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

NOTE: On Macs you can install Node with Brew:

```
brew install node
```

8. Yarn and JHipster:

NOTE: On Macs you can install Node with Brew:
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

# [TodoMVC](http://todomvc.com)
What is it? A frontend-only ToDo app implemented with in most of the popular JavaScript frameworks of today.

Let's deploy our own TodoMVC with S3 - look, no servers!

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
admin/admin
user/user

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

### Create Author data entity:

```
jhipster entity author
```

Create the following fields:
* name
 * Type: String
 * Validation: Yes
  * Select Required
 * No relationship
 * No DTO
 * No separate service class
 * Yes, with infinite scroll

### Commit code changes to local Git repo
```
// review updates in SourceTree
git add .
git status
git commit -m "Added Author entity"
```

### Create Book data entity:

```
jhipster entity book
```

Create the following fields:
* title
 * Type: String
 * Validation: Yes
  * Select Required
 * No DTO


### Now let's deploy it in our AWS EC2 instance:
Here we will explore two ways to deploy our app:

#### From the EC2 instance, checkout from Git repo, build and run

#### From your workstation, do a local build and upload binary package to EC2 instance


# [AWS CodeStar](https://aws.amazon.com/codestar/)
What is it? A unified user interface to quick-start and manage your software development activities in a single place.

# Want to learn more ?

[A Cloud Guru AWS Certified Solutions Architect](https://acloud.guru/learn/aws-certified-solutions-architect-associate)
   * Great exam preparation materials

[Cloud Academy AWS Certified Solutions Architect](https://cloudacademy.com/learning-paths/solutions-architect-associate-certification-preparation-for-aws-14/)
   * Good labs
