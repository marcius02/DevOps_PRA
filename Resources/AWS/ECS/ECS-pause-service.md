# Pause service

> We can pause a ECS servcie by setting the desired count of tasks in the service to zero. 

This effectively <mark>stops</mark> the service <mark>without deleting it</mark>, allowing you to easily restart it later by increasing the desired count.To pause an ECS service:

1. Use the AWS Management Console, AWS CLI, or AWS SDKs.
2. Navigate to the ECS service you want to pause.
3. Update the service and set the desired count to 0.

To resume the service, simply update the desired count to a number greater than zero.



Rewrite
