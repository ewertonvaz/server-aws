# Experiencias no Laravel 10
## Deploy na AWS

```
aws cloudformation deploy --stack-name laracasts --template-file $PWD/stack.json
aws cloudformation describe-stack-events --stack-name laracasts
aws cloudformation delete-stack --stack-name laracasts
```

https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-instances.html

```
aws ec2 describe-instances
aws ec2 describe-instances \
  --query 'Reservations[*].Instances[*].{Instance:InstanceId,Type:InstanceType,AZ:Placement.AvailabilityZone,IP_priv:PrivateIpAddress,IP_pub:PublicIpAddress,state:State.Name,key:KeyName }'
aws ec2 stop-instances --instance-ids <value>
aws ec2 start-instances --instance-ids <value>
```

### Deploy
```
ssh-keygen -f ~/.ssh/id_rsa -t rsa -P ""
cat ~/.ssh/id_rsa.pub
```
