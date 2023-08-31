# Snowflake-S3-Integration
DATA FLOW DIAGRAM <br>
![Alt text](https://github.com/ShreyaDasmahapatra/Snowflake-S3-Integration/blob/main/img/storage-integration-s3.png)
 

Objective: Read the file from S3 bucket and make it available in snowflake env. <br>
So to support the above we need the following: <br>

1.AWS S3 Bucket creation <br>
2.AWS permissions for authentication <br>
3.Integration object : A storage integration is a Snowflake object that stores a generated identity and access management (IAM) entity for your external cloud storage, along with an optional set of allowed or blocked storage locations <br>
4.External Stage Object: A storage integration is a Snowflake object that stores a generated identity and access management (IAM) entity for your external cloud storage, along with an optional set of allowed or blocked storage locations <br>

STEPs. <br>
1.Create S3 bucket :  <br>
&nbsp&nbsp&nbsp&nbspa.Give it a name and keep everything as default. <br>
					b.Upload file to the bucket <br>
					c.Open the file in S3.The following is the path of the file <br>
						S3 URI:  s3:// /snowflake-s3-shreya /netflix_titles.csv	<br>
2.Create permissions/Role so that 3rd party can communicate with AWS: <br>
					a.IAM>>policies>>create policy>>JSON>> copy paste json from S3-policy file from your local ,give it a name and create policies. <br>

                                        b. IAM>>policies>>create roles>>aws account>>attach the name to the policy created and <br>
 	                                 		i.aws account because we are giving access to 3rd party <br>
							ii. This account (516915275482)	<br>
							iii.Copy paste the above in the external ID>>attach the role to the policy created and create role>>click on to the role and copy the arn no. <br>									(arn:aws:iam::516915275482:role/snowflake-aws-shreya) <br>

3.Create database,schema,storage integration(refer snowflake.txt) <br>
					Like  AWS has created the arn similar to that snowflake will also create an external id and arn which needs to be communicated to aws. <br>
					Snowflake arn : arn:aws:iam::516915275482:role/snowflake-s3-shreya <br>
					Snowflake external Id : PC46691_SFCRole=2_i8FvJBQdxjklSfKAVRsDmjc7z/Y= <br>
4. Map 3rd party(snowflake) details to roles of AWS <br>
IAM>>Roles>>snowflake-aws-shreya>>Trust Relationship>>Edit trust policy>> add the above arn and external id to the code <br>






```
Trust Policy
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Principal": {
				"AWS": "arn:aws:iam::463934274011:user/o8eb0000-s"
			},
			"Action": "sts:AssumeRole",
			"Condition": {
				"StringEquals": {
					"sts:ExternalId": "PC46691_SFCRole=2_vpKEcDLEJFPf4hHD6/vbGlFhXnc="
				}
			}
		}
	]
}
```
<br>
						

5.Create External Stage : To load file from 3rd party we will be creating external stage but before that we need to create file format.<br>
	a.Create file format <br>
	b.Create external stage <br>
6.Create temporary table : To load data from files from s3 bucket into the table.


