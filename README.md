# ec2-delete-snapshots
Mass Delete of EC2 Snapshots via aws cli

# Why?
I came across a requirement to delete a bunch of old snapshots from a previous MSP who'd left behind their backups.

AWS Console allows you to do this, 50-100 at a time.  In this instance I was looking at thousands. 

# How?
A quick google showed many people had a way of doing this, some not as compatible as others.   Well, here's my take. 

# The "Source"

Using the aws cli in a bash shell, do the following.  You'll need;

 * A pattern, in this case, a description. 
 * Your region
 * an understanding of aws cli
 
 `aws ec2 describe-snapshots --filters Name="description",Values="AnOldMSPs-HourlySnapshot" --query "Snapshots[*].{ID:SnapshotId}" --region ap-southeast-2 --output text | awk '{print "Deleting -> " $1; system("aws ec2 delete-snapshot --snapshot-id " $1 " --region ap-southeast-2")}'`

# The explain bit....

 * `aws ec2 describe-snapshots` - list the snapshots availabe. 
 * `-- filters` - Restricting our sctions to certain snapshots, in this case on description.
 * `--query` - We only want to see the Snapshot ID in.
 * `--region` - The region our snapshots are sitting in
 * `--output` - IMPORTANT, needs to be text. 
 * `awk` - The command we'll use for output and execution of the delete command
 * `system` - Execution of the delete 
 * `aws ec2 delete-snapshot` - Delete the snapshot that we're dealing with. 
 
You can dry run this code by running it as below prior. 
 
 `aws ec2 describe-snapshots --filters Name="description",Values="AnOldMSPs-HourlySnapshot" --query "Snapshots[*].{ID:SnapshotId}" --region ap-southeast-2 --output text | awk '{print "Deleting -> " $1;}'`
 
This will then just output the list of snapshots that WOULD have been deleted. 

# References
https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-snapshots.html
https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-snapshot.html
https://forums.aws.amazon.com/thread.jspa?threadID=26955
