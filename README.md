<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Cloud Security with AWS IAM

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-iam)

**Author:** Amna Sajid  
**Email:** amnasajiddd@gmail.com

---

![Image](http://learn.nextwork.org/radiant_amber_brave_peafowl/uploads/aws-security-iam_1c864649)

---

## Introducing Today's Project!

Today, I will be using the AWS Identity and Access Management (IAM) service to control who is authenticated (signed in) and authorized (has permissions) in your AWS account.
I'll launch an EC2 instance, and then control who has access to it by creating some IAM policies and user groups.

### Tools and concepts

Services I used were AWS EC2 and AWS IAM, and IAM Policy Simulator. Key concepts I learnt include:
💻 Launching an EC2 instances.
🏷️ Using tags for easy identification.
💂 Setting up IAM policies accessing EC2 instances based on their environment (development or production).
👩‍👩‍👧‍👧 Creating an IAM user and assigning them to the appropriate user group with the necessary permissions for their role.
🔓 Testting IAM access for the User I created.



### Project reflection

This project took me approximately 1.5 hours today including demo time! The most challenging part was understanding IAM Policies since it was written in JSON and contained multiple statements. It was most rewarding to see permission denied when our intern tried to delete our production instance - our IAM Access management is working. 

---

## Tags

Tags are like labels you can attach to AWS resources for organization.

This tagging helps us with identifying all resources with the same tag at once (they are useful filters when you're searching for something), cost allocation, and applying policies based on environment types. 

In this case, I've created tags called "Env" with a value of "production" and "development" to label the instances used in production vs development environments.

![Image](http://learn.nextwork.org/radiant_amber_brave_peafowl/uploads/aws-security-iam_2e0e5a5d)

---

## IAM Policies

An IAM policy is a rule for who can do what with your AWS resources. It's all about giving permissions to IAM users, groups, or roles, saying what they can or can't do on certain resources, and when those rules kick in.

### The policy I set up

For this project, I’ve set up a policy using JSON.

I’ve created a policy that allows some actions (like starting, stopping, and describing EC2 instances) for instances tagged with "Env = development" while denying the ability to create or delete tags for all instances.



### When creating a JSON policy, you have to define its Effect, Action and Resource.

‍Effect
‍This can have two values - either Allow or Deny - to indicate whether the policy allows or denies a certain action. Deny has priority. Looking at the first statement, "Effect": "Allow" means this statement is trying to allow for an action.

‍Action
‍A list of the actions that the policy allows or denies. In this case, "Action": "ec2:*" means all actions that you could possibly take on EC2 instances are allowed. Woohoo!

‍Resource
‍Which resources does this policy apply to? Specifying "*" means all resources within the defined scope (see the next point).

Condition Block (optional)
‍The circumstances under which the policy is in action. In this case, the condition is that the resource is tagged Env - development. This means specifying "Resource": "*" in the line above means all resources with the Env - development tag are impacted by your statement.

---

## My JSON Policy

![Image](http://learn.nextwork.org/radiant_amber_brave_peafowl/uploads/aws-security-iam_1c864649)

---

## Account Alias

An Account Alias is a friendly name for your AWS account that you can use instead of your account ID (which is usually a bunch of digits) to sign in to the AWS Management Console.

Creating an account alias took me 2 min. Now, my new AWS console sign-in URL is https://nextwork-alias-apple.signin.aws.amazon.com/console

![Image](http://learn.nextwork.org/radiant_amber_brave_peafowl/uploads/aws-security-iam_0eb4439b)

---

## IAM Users and User Groups

### Users

An IAM user group is a collection/folder of IAM users. It allows you to manage permissions for all the users in your group at the same time by attaching policies to the group rather than individual users.

### User Groups

IAM user groups are the people who will get access to your resources/AWS account, whereas user groups are the collections/folders of users for easier user management.



I attached the policy I created to this user group, which means It allows me to manage permissions for all the users in your group at the same time rather than individual users.

---

## Logging in as an IAM User

The first way is by sending the email sign-in instructions and the second way is to share the .csv file that contains the user credentials. 

Once I logged in as my IAM user, I noticed that some of my dashboard panels were showing Access denied.  This was because as a new user, the AWS console will treat you as someone who is starting from 0 again.

![Image](http://learn.nextwork.org/radiant_amber_brave_peafowl/uploads/aws-security-iam_6f2ab446)

---

## Testing IAM Policies

I tested my JSON IAM policy by managing the instance state of the "production" environment. I tried to stop the "production" environment but it gave me an error because I did not have the policy permission to do so. Then I tried to manage the state of the "development" instance by stopping it. It worked because, in the JSON IAM policy, I have permission to stop the "development" instance. 

### Stopping the production instance

When I tried to stop the production instance an angry-looking banner told me I failed to stop this instance. The banner told me it was because I was not authorized! I don't have permission to stop any instance with the production tag.

![Image](http://learn.nextwork.org/radiant_amber_brave_peafowl/uploads/aws-security-iam_0e7a9d6a)

---

## Testing IAM Policies

### Stopping the development instance

Next, when I tried to stop the development instance it worked. This was because I have the permission in the JSON IAM policy.

![Image](http://learn.nextwork.org/radiant_amber_brave_peafowl/uploads/aws-security-iam_1811801c)

---

## The IAM Policy Simulator

In the last step, I ended up shutting down the development instance to test my intern's access. Shutting down EC2 instances could get pretty disruptive for your other engineers and your users, so it's best practice to run these tests in another way.

A special IAM tool called the Policy Simulator to validate policies without affecting your actual AWS resources. 

### How I used the simulator

I set up a simulation for the EC2 instance for "Deletetags" and "StopInstances". The results were denied because the intern did not have permission to delete tags or stop instances. Since the policy for stopping instances was explicitly denied I had to specify that I wanted to stop the "development" instance. 

![Image](http://learn.nextwork.org/radiant_amber_brave_peafowl/uploads/aws-security-iam_069d8a621)

---

---
