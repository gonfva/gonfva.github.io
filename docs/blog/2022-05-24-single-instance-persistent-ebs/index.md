---title: Single instance with persistent EBS
date: 2022-05-24T07:30:00Z
subtitle: Poor man autoscaling
categories: [Developer]
tags: [Developer]
---
## Single instance with persistent EBS

Sometimes you only want a single instance in AWS. But you know that everything fails, and you assume that your machine will fail too. You have your backups in place, but you have some data on disk that you would rather keep. In theory, that's what EBS are for. You detach your EBS volume, you destroy your instance, you create a new one, and you attach the volume.

But that is a faff.

If you want to automate all things, one possible solution is to have an autoscaling group, with minimum, maximum and desired of one instance, and in the cloud init process, you attach the EBS volume that you would have created elsewhere. 

How do you attach an EBS volume? Easy. First when you created the EBS, you assign it a specific tag. Then, in the cloud init process, you get your instance ID, and the volume ID of an EBS with the tag you've assigned. And finally you attach it. 

In code

```
INSTANCE_ID=$(TOKEN=`curl -s -X PUT "http://169.254.169.254/latest/api/token" \
    -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && curl -H "X-aws-ec2-metadata-token: $TOKEN" \
     -s http://169.254.169.254/latest/meta-data/instance-id)
VOLUME_ID=$(aws ec2 describe-volumes \
    --region eu-west-2 \
    --filters Name=tag:Name,Values=<name of volume> \
    --query Volumes[0].VolumeId --output text)
aws ec2 attach-volume --region eu-west-2 --device /dev/sdh --volume-id $VOLUME_ID --instance-id $INSTANCE_ID

until [[ $VOL_STATUS == '"in-use"' ]]; do
    VOL_STATUS=$(aws ec2 describe-volumes --region eu-west-2 --volume-ids $VOLUME_ID --query 'Volumes[0].State')
    sleep 5
done

```

Remember, that the instance profile will need to have a role able to do these two actions `ec2:DescribeVolumes` and       `ec2:AttachVolume`.

Finally, unrelated, you may want to have something like the following that both mounts, but format if needed

```
lsblk -f
FORMATED=$(sudo lsblk -f|grep nvme1n1| xargs)
if [ "$FORMATED" == "nvme1n1" ];
then
    echo "Formatting"
    mkfs -t ext4 /dev/nvme1n1
fi

mount /dev/nvme1n1 <mount path>
echo '/dev/nvme1n1  <mount path> ext4    defaults    0   1'|sudo tee -a /etc/fstab
```
