Git ARN HP Spectre Notes

from:
https://docs.gitlab.com/ee/README.html

https://docs.gitlab.com/ee/gitlab-basics/start-using-git.html
https://help.github.com/articles/setting-your-username-in-git/

set
    git config --global user.name "richardglenski"
   git config --global user.email "richard.glenski@gmail.com"


verify
    git config --global user.name
    git config --global user.email

list global options
    git config --global --list

view repos ( local )
    git remote -v

git clone 
from: 
https://askubuntu.com/questions/802239/how-to-clone-a-git-repository/802243

Running git clone https://github.com/<user name>/<repository name> clones the repository into a local directory that is also named <repository name>. Running the same command again gives the error you saw because there is already a non-empty directory named <repository name>.
To proceed, you have two options:
You can clone the repository into a different directory, which we call <different name>:
git clone https://github.com/<user name>/<repository name> <different name>
You can update the master branch of the cloned repository by running:
git fetch origin # fetch updates from origin remote 
git merge origin/master
Alternatively, you can combine the two commands above into one:
git pull origin master
NOTE:  To repull from master and overwrite any git tracked local changes:

git fetch --all
git reset --hard origin/master
git pull origin master


Ubuntu CLI
cp -r ~/stuff ~/backup/stuff-backup
 -v verbose flag
cp -vr stuff backup/stuff-backup


--------------------------

ls  /home/ubuntu/node/g48-notes-app-api
Ubuntu CLI
cp -r ~/stuff ~/backup/stuff-backup
 -v verbose flag
cp -vr stuff backup/stuff-backup

cp -r /home/ubuntu/node/g48-notes-app-api/*  /home/ubuntu/git/test2
cd /home/ubuntu/git/test/g48-notes-app-api
cd /home/ubuntu/git/test2

cp -r /home/ubuntu/node/g48-notes-app-api/*  /home/ubuntu/git/g48-notes-serverless

cd /home/ubuntu/git/g48-notes-serverless
sudo rm -r g48-notes-serverless


To update missing files

git checkout .

To repull from master and overwrite any git tracked local changes:

git fetch --all
git reset --hard origin/master
git pull origin master

( more info: https://stackoverflow.com/questions/1125968/how-do-i-force-git-pull-to-overwrite-local-files )

mv g48-notes-serverless g48-notes-serverless1


--------------------------

HP SPECTRE NOTES

ENG KYBD *WINDOWS + Space to switch

1234567890-=
!@#$%^&*()_+

qwertyuiop[]\
QWERTYUIOP{}|

asdfghjkl;'
ASDFGHJKL:"

zxcvbnm,./
ZXCVBNM<>?

ENG LLA

1234567890'¿
!"#$%&/()=?¡

qwertyuiop´+}
QWERTYUIOP¨*]

asdfghjklñ{
ASDFGHJKLÑ[]

zxcvbnm,.-
ZXCVBNM;:_


--------------------------

GENERAL AWS NOTES

ARN  -  Amazon Resource Names (ARNs) globally unique identifier  for AWS resources. 

3 ARN Formats.

arn:partition:service:region:account-id:resource
arn:partition:service:region:account-id:resourcetype/resource
arn:partition:service:region:account-id:resourcetype:resource


ARN EXAMPLES
<!-- Elastic Beanstalk application version -->
arn:aws:elasticbeanstalk:us-east-1:123456789012:environment/My App/MyEnvironment

<!-- IAM user name -->
arn:aws:iam::123456789012:user/David

<!-- Amazon RDS instance used for tagging -->
arn:aws:rds:eu-west-1:123456789012:db:mysql-db

<!-- Object in an Amazon S3 bucket -->
arn:aws:s3:::my_corporate_bucket/exampleobject.png


ARN USE CASE EXAMPLES:


Communication
 For example, you have an API Gateway listening for RESTful APIs and invoking the corresponding Lambda function based on the API path and request method. The routing looks like the following.

GET /hello_world => arn:aws:lambda:us-east-1:123456789012:function:lambda-hello-world
IAM Policy

IAM policy definition.

{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": ["s3:GetObject"],
    "Resource": "arn:aws:s3:::Hello-bucket/*"
}


