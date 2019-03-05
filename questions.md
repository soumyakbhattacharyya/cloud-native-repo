# Questions
## AWS
### IAM

* what are the services offered by IAM
* what are users, groups, policies and roles, in brief
* what is the sign in url; what is it being used for
* what is the root account
* which region is guarded by IAM
* what are the two kinds of permission that the newly created user has choise to be associated with
* how can we assert the restriction on the password policies
* what is user name, password, access key and secret access key
* what are roles
* why roles are being used for
* how can you create a new billing alarm using cloudwatch

#### Notes

* IAM consists of User, Groups, Roles and Policies
* Roles are for one AWS service to call another service
* Permission is granted through policies
* Policies are JSON document 
* IAM is universal; agnostic of any specific region
* root account is the one when you first set up AWS account
* new users have no permission by default
* new users get access key and secret key access key to be able to use the API
* always set up multi - factor authentication on root account
* it is possible to create and customize own password rotation policy
