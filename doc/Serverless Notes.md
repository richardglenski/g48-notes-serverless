serverless-stack.com Notebook Log Summary Redo


==============> https://serverless-stack.com/chapters/create-an-iam-user.html
 Setup AIM user for app to use

aws2018
Account ID:
	001222730153
AWS iam user 
appadmin
access key
AKIAJXHJBHB6DCM6GJRA
secret key
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

==============> https://serverless-stack.com/chapters/configure-the-aws-cli.html
( USE AWS Ubuntu VM )

Now  ubuntu on vm in AWS lightsail ( for AWS cli )

Connect using your own SSH client
You can connect to your instance using the following address and user name:
ivm Public IP  18.191.139.181
User name
ubuntu
Make StaticIP,, 13.58.19.88  (east ohio (east2)
login 
key file:
aws-ubuntu-lightsail-ohio-east2
setup and use putty, with putty conversion of key file
putty : left mouse select and copy, right paste(double click pad)

install aws cli

ubuntu@ip-172-26-12-71:~/python3$ aws configure
AWS Access Key ID [None]: AKIAJXHJBHB6DCM6GJRA
AWS Secret Access Key [None]: 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
Default region name [None]:
Default output format [None]:

==============> https://serverless-stack.com/chapters/create-a-dynamodb-table.html

Table name			notes
Primary partition key		userId (String)
Primary sort key		noteId (String)
Point-in-time recovery		DISABLED Enable
Encryption			DISABLED  (encrypt at rest - add later - FREE )
Time to live attribute		DISABLED Manage TTL
Table status			Active
Provisioned read capacity units		5 (Auto Scaling Enabled)
Provisioned write capacity units		5 (Auto Scaling Enabled)
Last decrease time		-
Last increase time		-
Storage size (in bytes)		0 bytes
Item count		0
Region		US East (Ohio)         "us-east-2"
Amazon Resource Name (ARN)}		arn:aws:dynamodb:us-east-2:001222730153:table/notes
change defaults  auto scale from scale at 70% , max 40000,40000 to scale at 80%, max 10,10

==============> https://serverless-stack.com/chapters/create-a-cognito-user-pool.html

 ----------------------  set up an S3 bucket to handle file uploads

name		g48.notes-app-bucket
no public access
no aws log access
default encrypt

---------------------- set CORS on

<CORSConfiguration>
	<CORSRule>
		<AllowedOrigin>*</AllowedOrigin>
		<AllowedMethod>GET</AllowedMethod>
		<AllowedMethod>PUT</AllowedMethod>
		<AllowedMethod>POST</AllowedMethod>
		<AllowedMethod>HEAD</AllowedMethod>
		<AllowedMethod>DELETE</AllowedMethod>
		<MaxAgeSeconds>3000</MaxAgeSeconds>
		<AllowedHeader>*</AllowedHeader>
	</CORSRule>
</CORSConfiguration>


------------------------ add cognito-user-pool: 

Pool Id			us-east-2_X254aYEE0
Pool ARN		arn:aws:cognito-idp:us-east-2:001222730153:userpool/us-east-2_X254aYEE0
Estimated number of users 		1
Required attributes 		none
Alias attributes 		none
Username attributes		 email, phone_number
Custom attributes 		Choose custom attributes...
Minimum password length	8
Password policy		 uppercase letters, lowercase letters, numbers
User sign ups allowed? 	Users can sign themselves up
MFA	 			Enable MFA...
Verifications 			Email
Advanced security 		Enable advanced security...
Tags 				Choose tags for your user pool
App clients 			g48-notes-app
Triggers 			Add triggers...

------------------------ add app client:

region:  east ohio
name:  g48-notes-app
App client id:   	1hid61j8vopgu4u8alsqbivf4o
App client secret ( no secret key )

 [   ] Generate client secret: user pool apps with a client secret are not supported by the JavaScript SDK. We need to un-select the option.
[X] Enable sign-in API for server-based authentication: required by AWS CLI when managing the pool users via command line interface. We will be creating a test user through the command line interface in the next chapter.
 [   ]  Only allow Custom Authentication (CUSTOM_AUTH_FLOW_ONLY)Learn more.
 [   ]  Enable username-password (non-SRP) flow for app-based authentication (USER_PASSWORD_AUTH)Learn more.

amazon cognito domain name
https://g48-notes-app.auth.us-east-2.amazoncognito.com


==============> https://serverless-stack.com/chapters/create-a-cognito-test-user.html

------------------------  Add User

$ aws cognito-idp sign-up \
  --region YOUR_COGNITO_REGION \
  --client-id YOUR_COGNITO_APP_CLIENT_ID \
  --username admin@example.com \
  --password Passw0rd!

$ aws cognito-idp sign-up \
  --region us-east-2\
  --client-id 1hid61j8vopgu4u8alsqbivf4o\
  --username notesapp-test2@richardg.fastmail.com \
  --password Password1234

ubuntu@ip-172-26-12-71:~$  aws cognito-idp sign-up \
>   --region us-east-2\
>   --client-id 1hid61j8vopgu4u8alsqbivf4o\
>   --username notesapp-test2@richardg.fastmail.com \
>   --password Password1234
{
    "CodeDeliveryDetails": {
        "DeliveryMedium": "EMAIL",
        "Destination": "n***@r***.com",
        "AttributeName": "email"
    },
    "UserSub": "749b434e-8d90-4f82-9c55-f4a1f7179540",
    "UserConfirmed": false
}

------------------------  Confirm User

aws cognito-idp admin-confirm-sign-up \
  --region YOUR_COGNITO_REGION \
  --user-pool-id YOUR_COGNITO_USER_POOL_ID \
  --username admin@example.com

aws cognito-idp admin-confirm-sign-up \
  --region us-east-2\
  --user-pool-id us-east-2_X254aYEE0\
  --username  notesapp-test2@richardg.fastmail.com


==============>  https://serverless-stack.com/chapters/configure-the-aws-cli.html

ubuntu@ip-172-26-12-71:~/python3$ aws configure
AWS Access Key ID [None]: AKIAJXHJBHB6DCM6GJRA
AWS Secret Access Key [None]: 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
Default region name [None]:us-east-2
Default output format [None]:

ubuntu@ip-172-26-12-71:~/git/g48-notes-serverless$ aws configure
AWS Access Key ID [****************GJRA]:
AWS Secret Access Key [****************ua+L]:
Default region name [us-east-2]:
Default output format [None]:

==============>  https://serverless-stack.com/chapters/setup-the-serverless-framework.html

------------------------ Create  Backend API's -AWS  Lambda, install node, install serverless 
 
------------------------  remove project files 
sudo rm -r  /home/ubuntu/gittemp1/g48-notes-serverless
sudo rm -r  /home/ubuntu/git2/g48-notes-serverless

------------------------  remove old node (IF NEEDED )  reference: https://nodejs.org/en/download/package-manager/


$ sudo apt-get remove nodejs npm

------------------------  Setup desired version of node to install ( in   /home/ubuntu )
$ cd  /home/ubuntu
$ curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
----------  then install node
$ sudo apt-get install -y nodejs
----------  then install serverless
$ sudo  npm install serverless -g

---------- then install serverless starter project to temp folder
$ mkdir  /home/ubuntu/gittemp2
$ cd  /home/ubuntu/gittemp2
$ serverless install --url https://github.com/AnomalyInnovations/serverless-nodejs-starter --name g48-notes-serverless 

----------  this makes 
/home/ubuntu/gittemp2/g48-notes-serverless 

----------  then
$ cd  /home/ubuntu/gittemp2/g48-notes-serverless 
$ sudo npm install
----------  Next, we’ll install a couple of other packages specifically for our backend.
$ sudo npm install aws-sdk --save-dev
$ sudo npm install uuid --save


------------------------  after install result:
node --version
serverless -version
aws --version

ubuntu@ip-172-26-12-71:~$ node --version
v8.11.4
ubuntu@ip-172-26-12-71:~$ pwd
/home/ubuntu
ubuntu@ip-172-26-12-71:~$ cd gittemp2
ubuntu@ip-172-26-12-71:~/gittemp2$ serverless -version
1.30.1
ubuntu@ip-172-26-12-71:~/gittemp2$
ubuntu@ip-172-26-12-71:~/gittemp2$ aws --version
aws-cli/1.15.81 Python/3.5.2 Linux/4.4.0-1052-aws botocore/1.10.80

note: maybe npm msg: update npm
$  npm i -g npm


------------------------  NOW UPDATE FROM Github repo
--------- list global GIT options
     $  git config --global --list
user.name=richardglenski
user.email=richard.glenski@gmail.com

--------- copy repo to TARGET folder
ubuntu@ip-172-26-12-71:~$ mkdir  /home/ubuntu/git1
cd  /home/ubuntu/git1
 git clone https://github.com/richardglenski/g48-notes-serverless.git

------------------------  verify files exist 
ls /home/ubuntu/git1/g48-notes-serverless
echo ------------------------------------
ls /home/ubuntu/gittemp2/g48-notes-serverless

------------------------ copy new git repo over existing basic install ( With -nr )
sudo cp -nr /home/ubuntu/gittemp2/g48-notes-serverless/*   /home/ubuntu/git1/g48-notes-serverless

------------------------  checking:

cd /home/ubuntu/git1/g48-notes-serverless
ls
git pull
( should reply up to date )

------------------------  move functions to    /home/ubuntu/git1/g48-notes-serverless/src
works to put src/list2.main in serverless.yml, en ves de list2.main

------------------------  SO now test on AWS VM:

cd /home/ubuntu/git1/g48-notes-serverless

serverless invoke local --function create --path mocks/create-event.json
serverless invoke local --function create --path mocks/create-event2.json

------------- this  attached tree.jpg
serverless invoke local --function create --path mocks/create-event-root-tree1.json
serverless invoke local --function create --path mocks/create-event-pic-tree1.json


serverless invoke local --function list --path mocks/list-event.json
serverless invoke local --function list2 --path mocks/list-event.json

-------------this needs specific note id in mocks/get-event.json
serverless invoke local --function get --path mocks/get-event-nopic.json
serverless invoke local --function get --path mocks/get-event-tree1.json
serverless invoke local --function get --path mocks/get-event-pics-tree1.json

serverless invoke local --function update --path mocks/update-event.json
serverless invoke local --function update --path mocks/update-event-treeroot2.json

-------------------------
ubuntu@ip-172-26-12-71:~/git1/g48-notes-serverless$ serverless invoke local --function get --path mocks/create-event-get-nopic.json
Serverless: Bundling with Webpack...
Time: 697ms
Built at: 2018-08-24 18:38:58
         Asset      Size   Chunks             Chunk Names
    src/get.js  10.2 KiB  src/get  [emitted]  src/get
src/get.js.map  7.41 KiB  src/get  [emitted]  src/get
Entrypoint src/get = src/get.js src/get.js.map
[./src/get.js] 2.57 KiB {src/get} [built]
[./src/libs/dynamodb-lib.js] 526 bytes {src/get} [built]
[./src/libs/response-lib.js] 762 bytes {src/get} [built]
[aws-sdk] external "aws-sdk" 42 bytes {src/get} [built]
[babel-runtime/core-js/json/stringify] external "babel-runtime/core-js/json/stringify" 42 bytes {src/get} [built]
[babel-runtime/helpers/asyncToGenerator] external "babel-runtime/helpers/asyncToGenerator" 42 bytes {src/get} [built]
[babel-runtime/regenerator] external "babel-runtime/regenerator" 42 bytes {src/get} [built]
[source-map-support/register] external "source-map-support/register" 42 bytes {src/get} [built]
Serverless: INVOKING INVOKE
{
    "statusCode": 200,
    "headers": {
        "Access-Control-Allow-Origin": "*",
        "Access-Control-Allow-Credentials": true
    },
    "body": "{\"content\":\"hello world no attachment\",\"createdAt\":1535135419859,\"noteId\":\"c1554230-a7cb-11e8-b32b-43c9d7fefa56\",\"userId\":\"USER-SUB-1234\"}"
}

ubuntu@ip-172-26-12-71:~/git1/g48-notes-serverless$ serverless invoke local --function get --path mocks/create-event-get-tree1.json
Serverless: Bundling with Webpack...
Time: 727ms
Built at: 2018-08-24 18:39:02
         Asset      Size   Chunks             Chunk Names
    src/get.js  10.2 KiB  src/get  [emitted]  src/get
src/get.js.map  7.41 KiB  src/get  [emitted]  src/get
Entrypoint src/get = src/get.js src/get.js.map
[./src/get.js] 2.57 KiB {src/get} [built]
[./src/libs/dynamodb-lib.js] 526 bytes {src/get} [built]
[./src/libs/response-lib.js] 762 bytes {src/get} [built]
[aws-sdk] external "aws-sdk" 42 bytes {src/get} [built]
[babel-runtime/core-js/json/stringify] external "babel-runtime/core-js/json/stringify" 42 bytes {src/get} [built]
[babel-runtime/helpers/asyncToGenerator] external "babel-runtime/helpers/asyncToGenerator" 42 bytes {src/get} [built]
[babel-runtime/regenerator] external "babel-runtime/regenerator" 42 bytes {src/get} [built]
[source-map-support/register] external "source-map-support/register" 42 bytes {src/get} [built]
Serverless: INVOKING INVOKE
{
    "statusCode": 200,
    "headers": {
        "Access-Control-Allow-Origin": "*",
        "Access-Control-Allow-Credentials": true
    },
    "body": "{\"content\":\"hello world with attachment\",\"attachment\":\"tree1.jpg\",\"createdAt\":1535135037463,\"noteId\":\"dd684270-a7ca-11e8-b6dc-a5c4628b4611\",\"userId\":\"USER-SUB-1234\"}"
}

ubuntu@ip-172-26-12-71:~/git1/g48-notes-serverless$ serverless invoke local --function get --path mocks/create-event-get-pics-tree1.json
Serverless: Bundling with Webpack...
Time: 709ms
Built at: 2018-08-24 18:39:05
         Asset      Size   Chunks             Chunk Names
    src/get.js  10.2 KiB  src/get  [emitted]  src/get
src/get.js.map  7.41 KiB  src/get  [emitted]  src/get
Entrypoint src/get = src/get.js src/get.js.map
[./src/get.js] 2.57 KiB {src/get} [built]
[./src/libs/dynamodb-lib.js] 526 bytes {src/get} [built]
[./src/libs/response-lib.js] 762 bytes {src/get} [built]
[aws-sdk] external "aws-sdk" 42 bytes {src/get} [built]
[babel-runtime/core-js/json/stringify] external "babel-runtime/core-js/json/stringify" 42 bytes {src/get} [built]
[babel-runtime/helpers/asyncToGenerator] external "babel-runtime/helpers/asyncToGenerator" 42 bytes {src/get} [built]
[babel-runtime/regenerator] external "babel-runtime/regenerator" 42 bytes {src/get} [built]
[source-map-support/register] external "source-map-support/register" 42 bytes {src/get} [built]
Serverless: INVOKING INVOKE
{
    "statusCode": 200,
    "headers": {
        "Access-Control-Allow-Origin": "*",
        "Access-Control-Allow-Credentials": true
    },
    "body": "{\"content\":\"hello world with attachment\",\"attachment\":\"pics/tree1.jpg\",\"createdAt\":1535135160815,\"noteId\":\"26ee47f0-a7cb-11e8-a7b0-6f64f11f1d1a\",\"userId\":\"USER-SUB-1234\"}"
}

ubuntu@ip-172-26-12-71:~/git1/g48-notes-serverless$


