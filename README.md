To avoid having full admin credentials on disk for long periods of time
I use this script to fetch full admin temporary credentials.

The script recreates the ~/.aws/config file to include the permanent profile (mfa protected IAM user) and a new default profile with the temporary credentials.

<user> - local user

<IAM_user> - should be replaced with an IAM mfa protected user
with appropriate permissions + preferably the following condition to force mfa authentication:

"Condition": {"Null": {"aws:MultiFactorAuthAge": "false"}}

I'm using a full admin user, if you a need less permissions remember to allow the user to assume the actual role.
 
000000000000 - should be replaced with the account ID.

<existing_role> - existing role with the needed permissions.
The role should include the account itself as a trusted entity.
Example policy to attach under Trust relationship:
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::000000000000:root"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
The first parameter is a duration one, should be between 900 seconds (15 minutes) - 3600 seconds (1 hour).
! For scripts that run longer than an hour use get-session-token
! temporary credentials that are fetched with get-session-token will not
allow to make IAM/STS calls.
Second parameter is the mfa token.




