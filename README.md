# spring-boot-oauth2-openidc

In this repository I am exloring the ways to enable SSO with the  OAuth2 provider - Pivotal UAA,  for Spring Boot 2 based applications.
Following are the instructions to bootstrap OpenID connect server  on a local machine using the Cloud Foundry UAA project. This repository is in WIP state.

### Start a local Pivotal UAA OAuth2 authorization server

git clone https://github.com/cloudfoundry/uaa

Run ./gradlew run . This will start the UAA server at your local at port 8080

### Install the UAA CLI 

https://github.com/cloudfoundry/cf-uaac 

### Target local UAA
uaac target http://localhost:8080/uaa --skip-ssl-validation

### Login as a canned client
uaac token client get admin -s adminsecret

### You can explore the token can using :

uaac token decode

### The raw jwt token can be obtained using the following command:

uaac context

### Add a client credential with client_id of client1 and client_secret of client1
uaac client add client1 \
   --name client1 \
   --scope resource.read,resource.write,openid,profile,email,address,phone \
   -s client1 \
   --authorized_grant_types authorization_code,refresh_token,client_credentials,password \
   --authorities uaa.resource \
   --redirect_uri http://localhost:8888/**


### Another client credential resource1/resource1
uaac client add resource1 \
  --name resource1 \
  -s resource1 \
  --authorized_grant_types client_credentials \
  --authorities uaa.resource


### Add a few users
uaac user add user1 -p user1 --emails user1@user1.com
uaac user add user2 -p user2 --emails user2@user2.com


### Add two scopes resource.read, resource.write
uaac group add resource.read
uaac group add resource.write

### Assign user1 both resource.read, resource.write scope:
uaac member add resource.read user1
uaac member add resource.write user1


### Assign user2 only resource.read scope:
uaac member add resource.read user1
uaac member add resource.write user1
