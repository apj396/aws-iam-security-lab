<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Cloud Security with AWS IAM

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-iam)

**Author:** Adwait Joshi  
**Email:** apjadwait.joshi93@gmail.com

---

![Image](http://learn.nextwork.org/intense_indigo_vibrant_tayberry/uploads/aws-security-iam_1c864649)

---

## Introducing Today's Project!

### Project overview

In this project, we will demonstrate how to use the AWS Identity and Access Management (IAM) service to control access and permission setting in our AWS account.
We're doing this project to learn about cloud security from absolute foundations. There are jobs called 'IAM Engineers' focused on the skills we're about to build today.

### Tools and concepts

Services I used were Amazon EC2 and AWS IAM. Key concepts I learnt include IAM users, policies, user groups and account aliases. I also learn how to use the Policy Simulator and how the JSON policies work. How to launch an instance, how to tag an instance, how to login as another user.

### Project reflection

This project took me approximately 1.5 hours. The most challenging part was understanding the IAM policy since it was written in JSON and it contained multiple statements. It was most rewarding to see permission denied when the intern tried to delete the production instance.

---

## Tags

### What I did in this step

In this step, I will launch two Amazon EC2 instances because we need to boost NextWork's computing power as we're expecting more users and traffic into our website over the break.

### Understanding tags

Tags are organisational tools that lets us label our resources. They are helpful for grouping resources, cost allocation and applying policies for all resources with the same tag.

### My tag configuration

The tag I’ve used on my EC2 instances is called Env, which stands for environment. The value I’ve assigned for our instances are production and development.

![Image](http://learn.nextwork.org/intense_indigo_vibrant_tayberry/uploads/aws-security-iam_2e0e5a5d)

---

## IAM Policies

### What I did in this step

In this step, I will use IAM policies to control the access level of a new NextWork intern because he should have access to the development environment (i.e. the development instance) but NOT to the production environment.

### Understanding IAM policies

IAM Policies are like rules that determine who can do what in the AWS account. I am using policies today to control who has access to the production/environment instance.

### The policy I set up

For this project, I’ve set up a policy using JSON.

### Policy effect

I've created a policy that allows the policy holder (i.e. the intern) to have permission to do anything he want to any instance tagged with "development". He can also see information for any instance, but he is denied access to deleting/creating tags for any instance as well.

### Understanding Effect, Action, and Resource

The Effect, Action, and Resource attributes of a JSON policy means whether or not the policy is allowing/denying action (i.e. Effect); what the policy holder can or cannot do (i.e. Action); and the specific AWS resources that the policy relates to (i.e. Resource).

---

## My JSON Policy

![Image](http://learn.nextwork.org/intense_indigo_vibrant_tayberry/uploads/aws-security-iam_1c864649)

---

## Account Alias

### What I did in this step

In this step, I will set up an Account Alias, which is like a nickname for the AWS account's console login. This is because an account alias makes it simpler for the users to login.

### Understanding account aliases

An account alias is simply a nickname for the AWS account. Instead of a long account ID, you can now reference your account alias instead.

### Setting up my account alias

Creating an account alias took 30 seconds - it's a simple configuration in the IAM dashboard. Now, the new AWS console sign-in URL uses the alias instead of my account ID.

![Image](http://learn.nextwork.org/intense_indigo_vibrant_tayberry/uploads/aws-security-iam_0eb4439b)

---

## IAM Users and User Groups

### What I did in this step

In this step, I will set up two IAM resources -IAM users, and IAM user groups. This is because IAM users are like logins for people that want to access our AWS account, while user groups are like folders to manage users that have the same level of access.

### Understanding user groups

IAM user groups are like folders that collect IAM users so that you can apply permission settings at the group level.

### Attaching policies to user groups

I attached the policy created to this user group, which means any user created inside this group will automatically get the permissions attached to the NextWorkDevEnvironmentPolicy IAM policy.

### Understanding IAM users

IAM users are people or entities that have access/can login to the AWS account.

---

## Logging in as an IAM User

### Sharing sign-in details

The first way is to email sign-in instructions to the user, while the second way is to download a .csv file with the sign-in details inside.

### Observations from the IAM user dashboard

Once I logged in as IAM user, I noticed that the user is already denied access to panels on the main AWS console dashboard. This was because the permissions was only set up to the development EC2 instance, so the intern wouldn't have access to even see anything else.

![Image](http://learn.nextwork.org/intense_indigo_vibrant_tayberry/uploads/aws-security-iam_6f2ab446)

---

## Testing IAM Policies

### What I did in this step

In this step, I will login to my AWS account as the intern and test access to the production and development instances because I want to make sure the intern doesn't have the ability to do anything that affect the production environment.

### Testing policy actions

I tested my JSON IAM policy by attempting to stop both the development and the production instances.

### Stopping the production instance

When I tried to stop the production instance, I was met with an error. This was because the production instance is tagged with the 'production' label, which is outside of the scope of the permission policy - interns are only allowed to do things to development instances.

![Image](http://learn.nextwork.org/intense_indigo_vibrant_tayberry/uploads/aws-security-iam_0e7a9d6a)

### Stopping the development instance

Next, when I tried to stop the development instance, I succesfully saw the instance state change to Stopping and then Stopped. This was because the permission policy allows the intern (i.e. users in the nextwork-dev-group) to stop instances. 

![Image](http://learn.nextwork.org/intense_indigo_vibrant_tayberry/uploads/aws-security-iam_1811801c)

---

## IAM Policy Simulator

To extend my project, I'm going to test my permission policies in safer and more controlled way - a tool called the IAM Policy Simulator,  I'm doing this because having to stop instances and logging into AWS accounts as other users is bit disruptive.

### Understanding the IAM Policy Simulator

The IAM Policy Simulator is a tool that let us simulate actions and test permission settings by defining a specific user/group/role and the action we want to test for. It's useful for saving time when testing permission settings. No more logging into another user or actually stopping resources.

### How I used the simulator

I set up a simulation for whether dev user group has permission to StopInstances or DeleteTags. The results were denied for both. I had to adjust the scope of the EC2 instances to ones that are tagged with "development". Once applied that tag, permission was allowed.

![Image](http://learn.nextwork.org/intense_indigo_vibrant_tayberry/uploads/aws-security-iam_069d8a621)

---
### Deep Dive: Understanding the JSON Policy
In this project, I moved beyond basic checkboxes to write a custom JSON (JavaScript Object Notation) policy. This is the language AWS uses to define "who can do what."

Breakdown of the Policy Logic
The policy I implemented uses a specific structure to enforce the Principle of Least Privilege:

Effect: Set to Allow, which explicitly grants the permission.

Action: I specified ec2:StopInstances and ec2:StartInstances. This means the user can turn the "development" servers on or off, but they cannot delete (terminate) them or create new ones.

Resource: Instead of using a wildcard (*), which would give access to every server in the account, I used Condition Tags.

The "Magic" of Condition Tags
The most critical part of my policy was this block:

JSON
"Condition": {
  "StringEquals": {
    "aws:ResourceTag/Env": "development"
  }
}


Why this matters:
This logic creates an automated security boundary. Even if a user has the "StopInstance" permission, AWS checks the Tag on the server first.

If the tag is Env: development, the action works.

If the tag is Env: production, the action is automatically Denied.

This demonstrates how Attribute-Based Access Control (ABAC) allows a company to scale security without needing to manually update permissions for every new server created.
---
---
