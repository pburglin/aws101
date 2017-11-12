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

7. [SourceTree](https://www.sourcetreeapp.com) (optional):
SourceTree is a graphical interface for Git. It is especialy helpful to visualize changes in filesets and repositories with multiple branches.

## Validating pre-reqs:
```
aws --version
aws-cli/1.11.97 Python/2.7.14 Darwin/17.2.0 botocore/1.5.60

java -version
java version "1.8.0_45"

git --version
git version 2.13.6 (Apple Git-96)
```

# [TodoMVC](http://todomvc.com)
What is it? A frontend-only ToDo app implemented with in most of the popular JavaScript frameworks of today.

Let's deploy our own TodoMVC with S3 - look, no servers!

# [JHipster](http://www.jhipster.tech)
What is it? A development platform to generate, develop and deploy Spring Boot + Angular Web applications and Spring microservices.

Let's build our sample BookMart app locally, and review key aspects of the app.

Now let's deploy it in our AWS EC2 instance:
Checkout from Git repo
Upload package


# [AWS CodeStar](https://aws.amazon.com/codestar/)
What is it? A unified user interface to quick-start and manage your software development activities in a single place.

# Want to learn more ?

[A Cloud Guru AWS Certified Solutions Architect](https://acloud.guru/learn/aws-certified-solutions-architect-associate)
   * Great exam preparation materials

[Cloud Academy AWS Certified Solutions Architect](https://cloudacademy.com/learning-paths/solutions-architect-associate-certification-preparation-for-aws-14/)
   * Good labs
