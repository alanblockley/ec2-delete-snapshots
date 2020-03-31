# ec2-delete-snapshots
Mass Delete of EC2 Snapshots via aws cli

# Why?
I came across a requirement to delete a bunch of old snapshots from a previous MSP who'd left behind their backups.

AWS Console allows you to do this, 50-100 at a time.  In this instance I was looking at thousands. 

# How?
A quick google showed many people had a way of doing this, some not as compatible as others.   Well, here's my take. 

# The "Source"

Using the aws cli in a bash shell, do the following.  You'll need;
-A pattern, in this case, a description. 
-Your region
-an understanding of aws cli



