Step-1
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/configuring-IMDS-existing-instances.html

Step-2
https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-spark-application.html#emr-spark-application-spark27


aws ec2 describe-instances --instance-ids i-0e3aaec2bbd619167

s3://myemrbucket43/script/mypythonscript.py

aws emr add-steps --cluster-id j-2CMIASGCJW822 --steps Type=Spark,Name="MySparkJob",ActionOnFailure=CONTINUE,Args=[--deploy-mode,cluster,--master,yarn,--conf,spark.yarn.submit.waitAppCompletion=true,s3://myemrbucket43/script/mypythonscript.py]

vi mypythonscript.py

spark-submit --master yarn ./mypythonscript.py


aws ec2 describe-instances --instance-ids i-0e3aaec2bbd619167

s3://myemrbucket43/script/

aws s3 cp enable_imdsv2.sh s3://myemrbucket43/script/

j-2CMIASGCJW822

aws emr add-steps --cluster-id j-2CMIASGCJW822 --steps Type=CUSTOM_JAR,Name="Enable IMDSv2",ActionOnFailure=CONTINUE,Jar=s3://elasticmapreduce/libs/script-runner/script-runner.jar,Args=["s3://myemrbucket43/script/"]

aws emr modify-cluster --cluster-id j-2CMIASGCJW822 --metadata-options State=applied,HttpTokens=required,HttpPutResponseHopLimit=1,HttpEndpoint=enabled,HttpProtocolIpv6=disabled,InstanceMetadataTags=disabled

aws emr modify-cluster --cluster-id j-2CMIASGCJW822 --no-termination-protected

aws emr add-steps --cluster-id j-2CMIASGCJW822 --steps Type=CUSTOM_JAR,Name="Enable IMDSv2",ActionOnFailure=CONTINUE,Jar=s3://elasticmapreduce/libs/script-runner/script-runner.jar,Args=["s3://myemrbucket43/script/mypythonscript.py"]

aws ec2 modify-instance-metadata-options \
    --instance-id i-0e3aaec2bbd619167 \
    --http-tokens required \
    --http-endpoint enabled
	
	
	Token - AQAEAJ_3EBPAivCFNpy4rPwX0wySZvxOtOEO7LtiPePFhVcK27MteQ==



---
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:DescribeInstances",
            "Resource": "*"
        }
    ]
}
---------

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:ModifyInstanceMetadataOptions",
            "Resource": "arn:aws:ec2:us-east-1:084849424173:instance/i-0e3aaec2bbd619167"
        }
    ]
}
-------------------

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "elasticmapreduce:DescribeCluster",
            "Resource": "arn:aws:elasticmapreduce:us-east-1:084849424173:cluster/j-2CMIASGCJW822"
        }
    ]
}
--------------------------------
