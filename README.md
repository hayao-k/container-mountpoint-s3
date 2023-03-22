# container-mountpoint-s3
This is a simple container image to verify the operation of [Mountpoint for Amazon S3](https://github.com/awslabs/mountpoint-s3) in a container environment.

Container image is also available in [ECR Public gallery](https://gallery.ecr.aws/x8j1s2l2/container-mountpoint-s3).

## Getting started
Image Build

```shell
docker image build -t mount-s3:latest .
```

Run

```
docker container run --privileged --rm -it mount-s3:latest bash
```

Enjoy

```
mount-s3-user@ce43831fda04:~$ sudo mount-s3 <bucket_name> /mnt --allow-other --region ap-northeast-1
mount-s3-user@ce43831fda04:~$ ls -l /mnt/test.json
-rw-r--r-- 1 root root 306424 Feb 21 02:42 /mnt/test.json
```

## EC2 on Docker Consideration
If using EC2 IAM roles for AWS credentials, increasing the IMDSv2 hop limit from 1 to 2 in the instance metadata options is [recommended](https://docs.aws.amazon.com/ja_jp/AWSEC2/latest/UserGuide/instancedata-data-retrieval.html#imds-considerations).

> In a container environment, if the hop limit is 1, the IMDSv2 response does not return because going to the container is considered an additional network hop. To avoid the process of falling back to IMDSv1 and the resultant delay, in a container environment we recommend that you set the hop limit to 2

```
aws ec2 modify-instance-metadata-options \
    --instance-id i-xxxxxxxxxxxxxxxxx \
    --http-put-response-hop-limit 2 \
    --http-endpoint enabled
```
