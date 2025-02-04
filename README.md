# AWS Lambda Function: Identifying Stale EBS Snapshots for Cost Optimization

This Lambda function is designed to identify and delete stale EBS snapshots in your AWS environment. Stale snapshots are EBS snapshots that are no longer associated with any active EC2 instance. By deleting these unused snapshots, you can reduce your AWS storage costs.

## Features

- **Identifies EBS snapshots** owned by the account (`OwnerIds: ['self']`).
- **Fetches a list of active EC2 instances** (running and stopped).
- **Checks if each snapshot is associated with a volume** and if that volume is attached to an active EC2 instance.
- **Deletes stale snapshots** that are not associated with any active EC2 instance or volume.

## Prerequisites

- An AWS account with appropriate IAM permissions:
  - `ec2:DescribeSnapshots`
  - `ec2:DescribeInstances`
  - `ec2:DescribeVolumes`
  - `ec2:DeleteSnapshot`
- AWS Lambda function set up in your desired region.
- Boto3 library installed (included by default in Lambda Python runtime).

## Lambda Function Code in the file


## How It Works

1. **List EBS Snapshots**: The function first retrieves all the EBS snapshots that belong to the account.
2. **List Active EC2 Instances**: It then retrieves all active EC2 instances (both running and stopped instances).
3. **Check Snapshot Attachments**: For each snapshot, the function checks if it is attached to a volume. If the volume is not attached to an active EC2 instance, the snapshot is marked as stale.
4. **Delete Stale Snapshots**: If a snapshot is found to be stale, it is deleted to free up storage and reduce costs.

## How to Deploy

1. **Create the Lambda Function**:
   - Open the **AWS Lambda Console**.
   - Click **Create function**.
   - Choose **Author from scratch**.
   - Select **Python 3.x** as the runtime.
   - Create a new role with the necessary permissions or choose an existing role that has the required permissions for `ec2:DescribeSnapshots`, `ec2:DescribeInstances`, `ec2:DescribeVolumes`, and `ec2:DeleteSnapshot`.

2. **Deploy the Lambda Function**:
   - Copy the code provided above into the Lambda function editor.
   - Click **Deploy** to save the function.

3. **Test the Function**:
   - Create a test event (for testing purposes) or trigger the function manually.
   - Ensure that the Lambda function is able to list snapshots and EC2 instances and delete stale snapshots.

4. **Set Up a CloudWatch Event Rule** (Optional):
   - You can set up an EventBridge or CloudWatch Events rule to trigger the Lambda function on a schedule, e.g., every day at 10 AM (as described in the Cron example).

   Use this cron expression for daily execution at 10 AM:

   ```
   cron(0 10 * * ? *)
   ```

## IAM Permissions Required

Ensure your Lambda function has the following permissions attached to its IAM role:

- **`ec2:DescribeSnapshots`**: To list all EBS snapshots.
- **`ec2:DescribeInstances`**: To list EC2 instances.
- **`ec2:DescribeVolumes`**: To describe volumes associated with snapshots.
- **`ec2:DeleteSnapshot`**: To delete stale EBS snapshots.

## Cost Optimization Benefits

- **Reduce Unused Storage Costs**: By automatically deleting stale EBS snapshots, you prevent unnecessary storage charges.
- **Automate Resource Cleanup**: This Lambda function automates the cleanup process, ensuring that snapshots that are no longer needed are promptly deleted.
- **Improve Resource Management**: By managing snapshots effectively, you ensure that you're only paying for the storage that you actually need.

## Troubleshooting

- Ensure that your IAM role has the necessary permissions.
- If you encounter issues with `ec2:DescribeVolumes`, verify that the volumes associated with snapshots still exist.
- You can test the function with different EC2 and EBS configurations to ensure it works as expected.

## Outputs


<img width="958" alt="Snapshot Available" src="https://github.com/user-attachments/assets/9d9ecbfe-8239-4948-83d5-e5c6e324e18b" />

<img width="958" alt="Deleted Stale Snapshot" src="https://github.com/user-attachments/assets/2559954b-27f0-4a7e-9063-2a0e9b569f6f" />

<img width="959" alt="Deleted Stale Snapshot in EC2 png " src="https://github.com/user-attachments/assets/ab463d0d-8344-44b7-84f8-fe317c988efe" />

<img width="959" alt="Snapshot and volume actve" src="https://github.com/user-attachments/assets/77077584-e014-4c6e-8656-bdef127df1c6" />

<img width="959" alt="Deleted Stale Snapshot which is attached to Volume but no active instances" src="https://github.com/user-attachments/assets/3efdacf1-c67c-4bfa-a273-6f92fa1a5d4a" />

<img width="959" alt="Snapshot got deleted " src="https://github.com/user-attachments/assets/b0eea39f-6c38-4077-b709-c283c8526035" />


## Conclusion

This Lambda function helps automate the deletion of stale EBS snapshots, reducing your AWS storage costs by ensuring that only necessary snapshots are retained. By scheduling it to run periodically, you can ensure continuous cost optimization for your AWS environment.
