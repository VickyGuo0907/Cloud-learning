# AWS Certified Developer Associate Practice Exams 2021

## AWS EC2 
[AWS EC2 Cheat Sheet](https://tutorialsdojo.com/amazon-elastic-compute-cloud-amazon-ec2/)


## AWS Lambda 

[AWS Lambda Cheat Sheet](https://tutorialsdojo.com/aws-lambda/)

* Environment Variables, Stage Variables, Layers, Alias

    1. **Environment variables** for Lambda functions enable you to dynamically pass settings to your function code and libraries, without making changes to your code. Environment variables are key-value pairs that you create and modify as part of your function configuration, using either the AWS Lambda Console, the AWS Lambda CLI or the AWS Lambda SDK. 

    2. **Stage Variables** is only available in API Gateway and not in Lambda.

    3. **Layers** is only used to pull in additional code and content in the form of layers. A layer is just a ZIP archive that contains libraries, a custom runtime, or other dependencies.

    4. **Aliases** is just like a pointer to a specific Lambda function version. By using aliases, you can access the Lambda function which the alias is pointing to, without the caller knowing the specific version the alias is pointing to.

* **Concurrent executions** refers to the number of executions of your function code that are happening at any given time. You can estimate the concurrent execution count, but the concurrent execution count will differ depending on whether or not your Lambda function is processing events from a poll-based event source.
By default, **AWS Lambda limits the total concurrent executions across all functions within a given region to 1000. This limit can be raised by requesting for AWS to increase the limit of the concurrent executions of your account.**
    1. You can use this formula to estimate the capacity used by your function:
        
        `concurrent executions = (invocations per second) x (average execution duration in seconds)`
    
    2. For example, consider a Lambda function that processes Amazon S3 events. Suppose that the Lambda function takes on average three seconds and Amazon S3 publishes 10 events per second. Then, you will have 30 concurrent executions of your Lambda function. See the calculation shown below to visualize the process:
      
        `(10 events per second) x (3 seconds average execution duration) =  30 concurrent executions`

    3. In this scenario, it is expected that the Lambda function takes an average of 100 seconds for every execution with 50 requests per second. Using the formula above, the function will have 5,000 concurrent executions.
        
        `(50 events per second) x (100 seconds average execution duration) =  5,000 concurrent executions`

* In the Invoke API, you have 3 options to choose from for the InvocationType:

    1. **RequestResponse (default)** – Invoke the function synchronously. Keep the connection open until the function returns a response or times out. The API response includes the function response and additional data.

    2. **Event** – Invoke the function asynchronously. Send events that fail multiple times to the function’s dead-letter queue (if it’s configured). The API response only includes a status code.

    3. **DryRun** – Validate parameter values and verify that the user or role has permission to invoke the function.

*  For a Lambda function, you can have two types of integration:

    1. **Lambda proxy integration:** If your API does not require content encoding or caching, you only need to set the integration’s HTTP method to POST, the integration endpoint URI to the ARN of the Lambda function invocation action of a specific Lambda function, and the credential to an IAM role with permissions to allow API Gateway to call the Lambda function on your behalf.

    2. **Lambda custom integration:** in addition to the proxy integration setup steps, you also specify how the incoming request data is mapped to the integration request and how the resulting integration response data is mapped to the method response.

## Amazon CloudWatch

[Aamazon CloudWatch](https://tutorialsdojo.com/amazon-cloudwatch/)

* When you create an alarm, you specify three settings to enable CloudWatch to evaluate when to change the alarm state:

    1. **Period** is the length of time to evaluate the metric or expression to create each individual data point for an alarm. It is expressed in seconds. If you choose one minute as the period, there is one datapoint every minute.

    2. **Evaluation Period** is the number of the most recent periods, or data points, to evaluate when determining alarm state.

    3. **Datapoints to Alarm** is the number of data points within the evaluation period that must be breaching to cause the alarm to go to the ALARM state. The breaching data points do not have to be consecutive, they just must all be within the last number of data points equal to Evaluation Period.

* CloudWatch stores data about a metric as a series of data points. Each data point has an associated time stamp.Each metric is one of the following:

    1. **Standard resolution**, with data having a one-minute granularity
    2. **High resolution**, with data at a granularity of one second

* By default, your instance is enabled for basic monitoring. You can optionally enable detailed monitoring. After you enable detailed monitoring, the Amazon EC2 console displays monitoring graphs with a 1-minute period for the instance. The following table describes basic and detailed monitoring for instances.

    1. **Basic**: Data is available automatically in **5-minute** periods at no charge.

    2. **Detailed**: Data is available in 1-minute periods for an additional cost. To get this level of data, you must specifically enable it for the instance. For the instances where you’ve enabled detailed monitoring, you can also get aggregated data across groups of similar instances.

## AWS X-Ray

[AWS X-Ray Cheat Sheets](https://tutorialsdojo.com/aws-x-ray/)

* AWS X-Ray, Amazon CloudWatch, Amazon Inspector, AWS CloudTrail

    1. **AWS X-Ray** You can use X-Ray to analyze both applications in development and in production, from simple three-tier applications to complex microservices applications consisting of thousands of services.AWS X-Ray works with Amazon EC2, Amazon EC2 Container Service (Amazon ECS), AWS Lambda, and AWS Elastic Beanstalk. You can use X-Ray with applications written in Java, Node.js, and .NET that are deployed on these services.
    2. **Amazon CloudWatch** is incorrect because although you can troubleshoot the issue by checking the logs, it is still better to use AWS X-Ray as it enables you to analyze and debug your serverless application more effectively.
    3. **Amazon Inspector** is incorrect because this is primarily used for EC2 and not for Lambda.
    4. **AWS CloudTrail** is incorrect because this will only enable you to track all API calls to your Lambda, DynamoDB, and SNS. It is still better to use AWS X-Ray to debug your application.

* Annotations, Metadata
    1. **Annotations** are simple key-value pairs that are indexed for use with `filter expressions`. Use annotations to record data that you want to use to group traces in the console, or when calling the `GetTraceSummaries` API. X-Ray indexes up to 50 annotations per trace.

    2. **Metadata** are key-value pairs with values of any type, including objects and lists, but that are not indexed. Use metadata to record data you want to store in the trace but don’t need to use for searching traces. You can view annotations and metadata in the segment or subsegment details in the X-Ray console.

*  Enable the X-Ray daemon by including the `xray-daemon.config` configuration file in the `.ebextensions` directory of your source code.

* AWS Lambda uses environment variables to facilitate communication with the X-Ray daemon and configure the X-Ray SDK.

    1. **_X_AMZN_TRACE_ID**: Contains the tracing header, which includes the sampling decision, trace ID, and parent segment ID. If Lambda receives a tracing header when your function is invoked, that header will be used to populate the _X_AMZN_TRACE_ID environment variable. If a tracing header was not received, Lambda will generate one for you.

    2. **AWS_XRAY_CONTEXT_MISSING**: The X-Ray SDK uses this variable to determine its behavior in the event that your function tries to record X-Ray data, but a tracing header is not available. Lambda sets this value to `LOG_ERROR` by default.

    3. **AWS_XRAY_DAEMON_ADDRESS**: This environment variable exposes the X-Ray daemon’s address in the following format: `IP_ADDRESS:PORT`. You can use the X-Ray daemon’s address to send trace data to the X-Ray daemon directly, without using the X-Ray SDK.

* To properly instrument your application hosted in an EC2 instance, you have to install the **X-Ray daemon** by using a user data script. This will install and run the daemon automatically when you launch the instance. To use the daemon on Amazon EC2, create a new instance profile role or add the managed policy to an existing one. This will grant the daemon permission to upload trace data to X-Ray.**The AWS X-Ray daemon is a software application that listens for traffic on UDP port 2000, gathers raw segment data, and relays it to the AWS X-Ray API. The daemon works in conjunction with the AWS X-Ray SDKs and must be running so that data sent by the SDKs can reach the X-Ray service.**

## Amazon API Gateway

[AWS API Gateway Cheat Sheets](https://tutorialsdojo.com/amazon-api-gateway/)

* The metrics reported by API Gateway provide information that you can analyze in different ways. The list below shows some common uses for the metrics. These are suggestions to get you started, not a comprehensive list.

    1. Monitor the IntegrationLatency metrics to measure the responsiveness of the backend.
    2. Monitor the Latency metrics to measure the overall responsiveness of your API calls.
    3. Monitor the CacheHitCount and CacheMissCount metrics to optimize cache capacities to achieve a desired performance.

* A client of your API can invalidate an existing cache entry and reload it from the integration endpoint for individual requests. The client must send a request that contains the `Cache-Control: max-age=0` header.Ticking the `Require authorization` checkbox ensures that not every client can invalidate the API cache. If most or all of the clients invalidate the API cache, this could significantly increase the latency of your API.

* You can integrate an API method in your API Gateway with a custom HTTP endpoint of your application in two ways:

    1. HTTP proxy integration --- `HTTP_PROXY`
    2. HTTP custom integration --- `HTTP`

* Programmatically, you choose an integration type by setting the type property on the Integration resource. For the Lambda proxy integration, the value is `AWS_PROXY`. For the Lambda custom integration and all other `AWS` integrations, it is AWS. For the `HTTP` proxy integration and `HTTP` integration, the value is `HTTP_PROXY` and `HTTP`, respectively. For the mock integration, the type value is `MOCK`.

* For the integration timeout, the range is from `50 milliseconds to 29 seconds` for all integration types, including Lambda, Lambda proxy, HTTP, HTTP proxy, and AWS integrations.The following are the Gateway response types which are associated with the HTTP 504 error in API Gateway:

    1. **INTEGRATION_FAILURE** – The gateway response for an integration failed error. If the response type is unspecified, this response defaults to the DEFAULT_5XX type.

    2. **INTEGRATION_TIMEOUT** – The gateway response for an integration timed out error. If the response type is unspecified, this response defaults to the DEFAULT_5XX type.

* Amazon API Gateway Lambda proxy integration is a simple, powerful, and nimble mechanism to build an API with a setup of a single API method.

    1. **there is an incompatible output returned from a Lambda proxy integration backend.**: the Lambda function returns the result in XML format, it will cause the 502 errors in the API Gateway.
    2. **There has been an occasional out-of-order invocation due to heavy loads**:because although this is a valid cause of a 502 error, the issue is most likely caused by the Lambda function’s XML response instead of JSON.
    3. **The endpoint request timed-out**: this will likely result in 504 errors.

## Amazon VPC 
[Amazon VPC Cheat Sheets](https://tutorialsdojo.com/amazon-vpc/)

* **VPC Flow Logs** is a feature that enables you to capture information about the IP traffic going to and from network interfaces in your VPC. Flow log data can be published to Amazon CloudWatch Logs and Amazon S3.

* A route table contains a set of rules, called routes, that are used to determine where network traffic is directed.**Each subnet in your VPC must be associated with a route table; the table controls the routing for the subnet. A subnet can only be associated with one route table at a time, but you can associate multiple subnets with the same route table.**

Thus, the correct answer is a subnet can only be associated with one route table at a time.
## Amazon CloudFront

[Amazon CloudFront Cheat Sheet](https://tutorialsdojo.com/amazon-cloudfront/)

* CloudFront HTTP/S
You can configure one or more cache behaviors in your CloudFront distribution to require HTTPS for communication between viewers and CloudFront. You also can configure one or more cache behaviors to allow both HTTP and HTTPS, so that CloudFront requires HTTPS for some objects but not for others.To implement this setup, you have to change the **Origin Protocol Policy** setting for the applicable origins in your distribution. If you’re using the domain name that CloudFront assigned to your distribution, such as dtut0rial5d0j0.cloudfront.net, you change the **Viewer Protocol Policy** setting for one or more cache behaviors to require HTTPS communication. With this configuration, CloudFront provides the SSL/TLS certificate.



## Amazon S3

[Amazon S3 Cheat Sheet](https://tutorialsdojo.com/amazon-s3/)

* Server-side encryption protects data at rest. If you use Server-Side Encryption with Amazon S3-Managed Encryption Keys (SSE-S3), Amazon S3 will encrypt each object with a unique key and as an additional safeguard, it encrypts the key itself with a master key that it rotates regularly. Amazon S3 server-side encryption uses one of the strongest block ciphers available, 256-bit Advanced Encryption Standard (AES-256), to encrypt your data.`x-amz-server-side-encryption` header to request server-side encryption.

* server-side encryption with customer-provided encryption keys (SSE-C), you must provide encryption key information using the following request headers:
    1. `x-amz-server-side​-encryption​-customer-algorithm`
    2. `x-amz-server-side​-encryption​-customer-key`
    3. `x-amz-server-side​-encryption​-customer-key-MD5`

* since the `put-bucket-policy` command can only be used to apply policy at the bucket level, not on objects. **You can use S3 Access Control Lists (ACLs) instead to manage permissions of S3 objects.**

## AWS CodeCommit

[AWS CodeCommit Cheat Sheet](https://tutorialsdojo.com/aws-codecommit/)

* In CodeCommit, the primary resource is a repository. Each resource has a unique Amazon Resource Names (ARN) associated with it. In a policy, you use an Amazon Resource Name (ARN) to identify the resource that the policy applies to. To manage access to AWS resources, you attach permissions policies to IAM identities. You use identity-based policies to control access to repositories.

    1. The `codecommit:CreateRepository` and `codecommit:DeleteRepository` permissions enable you to create and delete a CodeCommit repository respectively. Hence, these two permissions are the correct permissions that should be given to the technical manager.

    2. `codecommit:GitPull` and `codecommit:GitPush` permissions are incorrect because the codecommit:GitPull permission simply pulls information from a CodeCommit repository to a local repo and the codecommit:GitPush permission is just used to push information from a local repo to a CodeCommit repository.

    3. `codecommit:CreateBranch` and `codecommit:DeleteBranch` permissions are incorrect because these enable you to create and delete a specific branch and not a repository. For this scenario, you have to use the codecommit:CreateRepository and codecommit:DeleteRepository permissions instead.

    4. `codecommit:*` is incorrect because although this permission will allow all actions to the code repository. This entails a security risk and violates the standard security practice of granting only the permissions required to perform certain tasks.


## AWS CodeDeploy

[AWS CodeDeploy Cheat Sheet](https://tutorialsdojo.com/aws-codedeploy/)

* CodeDeploy is a deployment service that automates application deployments to Amazon EC2 instances, on-premises instances, serverless Lambda functions, or Amazon ECS services.

    1. **Canary** deployment configuration, the traffic is shifted in two increments. You can choose from predefined canary options that specify the percentage of traffic shifted to your updated Lambda function version in the first increment and the interval, in minutes, before the remaining traffic is shifted in the second increment. Hence, this is the correct answer which will satisfy the requirement for the given scenario.
    2. **Linear**  will cause the traffic to be shifted in equal increments with an equal number of minutes between each increment. You can choose from predefined linear options that specify the percentage of traffic shifted in each increment and the number of minutes between each increment.
    3. **All-at-once** with this deployment configuration, the traffic is shifted from the original Lambda function to the updated Lambda function version all at once.
    4. **Rolling with additional batch** this is only applicable in Elastic Beanstalk and not for Lambda.

* CodeDeploy provides two deployment type options:

    1. **In-place deployment:** The application on each instance in the deployment group is stopped, the latest application revision is installed, and the new version of the application is started and validated. You can use a load balancer so that each instance is deregistered during its deployment and then restored to service after the deployment is complete. Only deployments that use the EC2/On-Premises compute platform can use in-place deployments. AWS Lambda compute platform deployments cannot use an in-place deployment type.

    2. **Blue/green deployment:** The behavior of your deployment depends on which compute platform you use:

        – Blue/green on an EC2/On-Premises compute platform: The instances in a deployment group (the original environment) are replaced by a different set of instances (the replacement environment). If you use an EC2/On-Premises compute platform, be aware that blue/green deployments work with Amazon EC2 instances only.

        – Blue/green on an AWS Lambda compute platform: Traffic is shifted from your current serverless environment to one with your updated Lambda function versions. You can specify Lambda functions that perform validation tests and choose the way in which the traffic shift occurs. All AWS Lambda compute platform deployments are blue/green deployments. For this reason, you do not need to specify a deployment type.

        – Blue/green on an Amazon ECS compute platform: Traffic is shifted from the task set with the original version of a containerized application in an Amazon ECS service to a replacement task set in the same service. The protocol and port of a specified load balancer listener are used to reroute production traffic. During deployment, a test listener can be used to serve traffic to the replacement task set while validation tests are run.

* The CodeDeploy agent is a software package that, when installed and configured on an instance, makes it possible for that instance to be used in CodeDeploy deployments. The CodeDeploy agent communicates outbound using HTTPS over port 443. CodeDeploy agent is required only if you deploy to an EC2/On-Premises compute platform. The agent is not required for deployments that use the Amazon ECS or AWS Lambda compute platform.

## Amazon DynamoDB

[Amazon DynamoDB Cheat Sheet](https://tutorialsdojo.com/amazon-dynamodb/)

* In general, `Scan` operations are less efficient than other operations in DynamoDB. A `Scan` operation always scans the entire table or secondary index. If possible, you should avoid using a `Scan` operation on a large table or index with a filter that removes many results. Also, as a table or index grows, the `Scan` operation slows. The `Scan` operation examines every item for the requested values and can use up the provisioned throughput for a large table or index in a single operation. For faster response times, design your tables and indexes so that your applications can use `Query` instead of `Scan`. For tables, you can also consider using the `GetItem` and `BatchGetItem` APIs.

* **Time To Live (TTL)** for DynamoDB allows you to define when items in a table expire so that they can be automatically deleted from the database.TTL is provided at no extra cost as a way to reduce storage usage and reduce the cost of storing irrelevant data without using provisioned throughput. With TTL enabled on a table, you can set a timestamp for deletion on a per-item basis, allowing you to limit storage usage to only those records that are relevant.

* **DynamoDB Streams** enables solutions such as these, and many others. DynamoDB Streams captures a time-ordered sequence of item-level modifications in any DynamoDB table, and stores this information in a log for up to 24 hours. Applications can access this log and view the data items as they appeared before and after they were modified, in near real time.A DynamoDB stream is an ordered flow of information about changes to items in an Amazon DynamoDB table. When you enable a stream on a table, DynamoDB captures information about every modification to data items in the table.

* **Amazon DynamoDB** is also integrated with AWS Lambda so that you can create triggers—pieces of code that automatically respond to events in DynamoDB Streams. With triggers, you can build applications that react to data modifications in DynamoDB tables.When an item in the table is modified, StreamViewType determines what information are written to the stream for this table. Valid values for StreamViewType are `KEYS_ONLY`, `NEW_IMAGE`, `OLD_IMAGE`, and `NEW_AND_OLD_IMAGES`.

    1. **OLD_IMAGE** type, the entire item which has the previous value as it appeared before it was modified is written to the stream.
    2. **KEYS_ONLY** type, it will only write the key attributes of the modified item to the stream. 
    3. **NEW_IMAGE** type, it will configure the stream to write the entire item with its new value as it appears after it was modified. 
    4. **NEW_AND_OLD_IMAGES** type, it will send both the new and the old item images of the item to the stream 

* **Amazon DynamoDB Accelerator (DAX)** is a fully managed, highly available, in-memory cache for DynamoDB that delivers up to a 10x performance improvement – from milliseconds to **microseconds**

* DynamoDB supports two types of secondary indexes:

    1. **Global secondary index** — an index with a partition key and a sort key that can be different from those on the base table. A global secondary index is considered “global” because queries on the index can span all of the data in the base table, across all partitions.
    2. **Local secondary index** — an index that has the same partition key as the base table, but a different sort key. A local secondary index is “local” in the sense that every partition of a local secondary index is scoped to a base table partition that has the same partition key value.

* **atomic counter** — a numeric attribute that is incremented, unconditionally, without interfering with other write requests. (All write requests are applied in the order in which they were received). With an atomic counter, the updates are not idempotent. In other words, the numeric value will increment each time you call UpdateItem. **conditional update** - where overcounting or undercounting cannot be tolerated (For example, in a banking application). In this case, it is safer to use a conditional update instead of an atomic counter.

* To return the number of write capacity units consumed by any of these operations, set the **ReturnConsumedCapacity** parameter to one of the following:

    1. **TOTAL** — returns the total number of write capacity units consumed.

    2. **INDEXES** — returns the total number of write capacity units consumed, with subtotals for the table and any secondary indexes that were affected by the operation.

    3. **NONE** — no write capacity details are returned. (This is the default.)

* **Local Secondary Index((LSI) - You cannot add a local secondary index to an existing table, nor can you delete any local secondary indexes that currently exist.**
* **Global Secondary Index (GSI) - You could add global secondary index to an existing table.**

* **Local secondary indexes are created at the same time that you create a table. You cannot add a local secondary index to an existing table, nor can you delete any local secondary indexes that currently exist.**

* [Global Secondary Index vs. Local Secondary Index](https://tutorialsdojo.com/global-secondary-index-vs-local-secondary-index/)

## AWS Elastic Beanstalk

[AWS Elastic Beanstalk Cheat Sheets](https://tutorialsdojo.com/aws-elastic-beanstalk/)

* In ElasticBeanstalk, you can choose from a variety of deployment methods:

    1. **All at once** – Deploy the new version to all instances simultaneously. All instances in your environment are out of service for a short time while the deployment occurs.
    2. **Rolling** – Deploy the new version in batches. Each batch is taken out of service during the deployment phase, reducing your environment’s capacity by the number of instances in a batch.
    3. **Rolling with additional batch** – Deploy the new version in batches, but first launch a new batch of instances to ensure full capacity during the deployment process.
    4. **Immutable** – Deploy the new version to a fresh group of instances by performing an immutable update.
    5. **Blue/Green** – Deploy the new version to a separate environment, and then swap CNAMEs of the two environments to redirect traffic to the new version instantly.

* In Elastic Beanstalk, 
    1. **env.yaml**: you can include a YAML formatted environment manifest in the root of your application source bundle to configure the environment name, solution stack and environment links to use when creating your environment. An environment manifest uses the same format as Saved Configurations.
    2. **Dockerrun.aws.json**:this configuration file is primarily used in multicontainer Docker environments that are hosted in Elastic Beanstalk. This can be used alone or combined with source code and content in a source bundle to create an environment on a Docker platform.
    3. **env.config**: this is just a custom configuration file which is not readily available in Elastic Beanstalk. Configuration files are YAML- or JSON-formatted documents with a .config file extension that you place in a folder named .ebextensions and deploy in your application source bundle. The more appropriate configuration file to use here is the env.yaml which can help you configure the environment name, solution stack, and environment links to use when creating your environment.
    4. **cron.yaml**: this configuration file is primarily used to define periodic tasks that add jobs to your worker environment’s queue automatically at a regular interval.


## Amazon ECS

[Amazon ECS Cheat Sheet](https://tutorialsdojo.com/amazon-elastic-container-service-amazon-ecs/)

* **A task placement strategy** is an algorithm for selecting instances for task placement or tasks for termination. Task placement strategies can be specified when either running a task or creating a new service.Amazon ECS supports the following task placement strategies:

    1. **binpack** – Place tasks based on the least available amount of CPU or memory. This minimizes the number of instances in use.
    2. **random** – Place tasks randomly.
    3. **spread** – Place tasks evenly based on the specified value. Accepted values are attribute key-value pairs, instanceId, or host.

* The `Random task` placement strategy is fairly straightforward as it doesn’t require further parameters. The two other strategies, such as `binpack` and `spread`, take opposite actions. Binpack places tasks on as few instances as possible, helping to optimize resource utilization, while spread places tasks evenly across your cluster to help maximize availability. By default, ECS uses spread with the ecs.availability-zone attribute to place tasks. Random places tasks on instances at random yet still honors the other constraints that you specified, implicitly or explicitly. Specifically, it still makes sure that tasks are scheduled on instances with enough resources to run them.

* Port mappings allow containers to access ports on the host container instance to send or receive traffic. Port mappings are specified as part of the container definition which can be configured in the task definition.For task definitions that use the `awsvpc` network mode, you should only specify the `containerPort`. The `hostPort` can be left blank or it must be the same value as the `containerPort`.

## AWS (Serverless Application Model) SAM

* A serverless application can include one or more nested applications.
    1. `AWS::Serverless::Application` -- define a nested application in your serverless application.
    2. `AWS::Serverless::Function` -- describes configuration information for creating a Lambda function.
    3. `AWS::Serverless::LayerVersion` -- creates a Lambda layer version (LayerVersion)
    4. `AWS::Serverless::Api` -- describes an API Gateway resource


## Amazon Elasticache 

[Amazon Elasticache Cheat Sheet](https://tutorialsdojo.com/amazon-elasticache/)

* Amazon ElastiCache is an in-memory key/value store that sits between your application and the data store (database) that it accesses. Whenever your application requests data, it first makes the request to the ElastiCache cache.
    1. **Lazy Loading** as its name implies, is a caching strategy that loads data into the cache only when necessary.
    2. **Write Through**This strategy adds data or updates data in the cache whenever data are written to the database, and not on the event of a cache miss.
    3. **Adding TTL**just adds a time to live (TTL) value to each write operation, which will eventually expire the cache object.


## Amazon KMS 

* The `GenerateDataKeyWithoutPlaintext` API generates a unique data key. This operation returns a data key that is encrypted under a customer master key (CMK) that you specify. `GenerateDataKeyWithoutPlaintext` is identical to `GenerateDataKey` except that it returns only the encrypted copy of the data key.

* `GenerateDataKey` this operation also returns a plaintext copy of the data key along with the copy of the encrypted data key under a customer master key (CMK) that you specified. Take note that the scenario explicitly mentioned that the API must return **only** the encrypted copy of the data key which will be used later for encryption. Although this API can be used in this scenario, it is not recommended since the actual encryption process of the data happens at a later time and not in real-time.

## Amazon SQS

[Amazon SQS Cheat Sheet](https://tutorialsdojo.com/amazon-sqs/)

* **Amazon SQS long polling** is a way to retrieve messages from your Amazon SQS queues. While the regular short polling returns immediately, even if the message queue being polled is empty, long polling doesn’t return a response until a message arrives in the message queue, or the long poll times out.Long polling helps reduce the cost of using Amazon SQS by eliminating the number of empty responses (when there are no messages available for a ReceiveMessage request) and false empty responses (when messages are available but aren’t included in a response). This type of polling is suitable if the new messages that are being added to the SQS queue arrive less frequently.

## Amazon SNS

[Amazon SNS Cheat Sheet](https://tutorialsdojo.com/amazon-sns/)

* Amazon Simple Notification Service (Amazon SNS) is a web service that coordinates and manages the delivery or sending of messages to subscribing endpoints or clients.With Amazon SNS, you have the ability to send push notification messages directly to apps on mobile devices. Push notification messages sent to a mobile endpoint can appear in the mobile app as message alerts, badge updates, or even sound alerts.
    1. **SQS**  this service is mainly used for message queues and not for notifications.
    2. **SES**  this service is mainly used for sending emails and not for notifications.

## Amazon Kinesis

[Amazon Kinesis Cheat Sheet](https://tutorialsdojo.com/amazon-kinesis/)

* when you use the Kinesis Client Library (KCL), you should ensure that the number of instances does not exceed the number of shards (except for failure standby purposes). Each shard is processed by exactly one KCL worker and has exactly one corresponding record processor, so you never need multiple instances to process one shard.

* **Amazon Kinesis Data Streams** supports changes to the data record retention period of your stream. A Kinesis data stream is an ordered sequence of data records meant to be written to and read from in real time. Data records are therefore stored in shards in your stream temporarily. The time period from when a record is added to when it is no longer accessible is called the retention period. **A Kinesis data stream stores records from 24 hours by default, up to 168 hours.**

## AWS Systems Manager

[AWS Systems Manager Cheat Sheet](https://tutorialsdojo.com/aws-systems-manager/)

* **AWS Systems Manager Parameter Store** provides secure, hierarchical storage for configuration data management and secrets management. You can store data such as passwords, database strings, and license codes as parameter values. You can store values as plain text or encrypted data. You can then reference values by using the unique name that you specified when you created the parameter.

## AWS Secrets Manager

[AWS Secrets Manager Cheat Sheet](https://tutorialsdojo.com/aws-secrets-manager/)

* you can configure Secrets Manager to automatically rotate the secret for you according to a schedule that you specify. 

## AWS IAM

[AWS IAM Cheat Sheet](https://tutorialsdojo.com/aws-identity-and-access-management-iam/)


## Amazon QuickSight

[Amazon QuickSight Cheat Sheet](https://tutorialsdojo.com/amazon-quicksight/)

* **Amazon QuickSight** is a fast, cloud-powered business intelligence service that makes it easy to deliver insights to everyone in your organization. As a fully managed service, QuickSight lets you easily create and publish interactive dashboards that include ML Insights. Dashboards can then be accessed from any device and embedded into your applications, portals, and websites.

## Amazon S3 Glacier

[Amazon S3 Glacier Cheat Sheet](https://tutorialsdojo.com/amazon-glacier/)

* There are three options for retrieving data with varying access times and cost:

    1. **Standard retrievals** allow you to access any of your archives within several hours. Standard retrievals typically complete within 3–5 hours. This is the default option.

    2. **Bulk retrievals** are Glacier’s lowest-cost retrieval option, which you can use to retrieve large amounts, even petabytes, of data inexpensively in a day. Bulk retrievals typically complete within 5–12 hours.

    3. **Expedited retrievals** allow you to quickly access your data when occasional urgent requests for a subset of archives are required. Expedited retrievals are typically made available within 1–5 minutes

## AWS Elastic Load Balancing

[AWS Elastic Load Balancing Cheat Sheet](https://tutorialsdojo.com/aws-elastic-load-balancing-elb/)

* **Application Load Balancers** provide two advanced options that you may want to configure when you use ALBs with AWS Lambda: 
    1. support for multi-value headers 
    2. health check configurations. 

## Comparison of AWS Services

[Comparison of AWS Services Cheat Sheets](https://tutorialsdojo.com/comparison-of-aws-services-for-udemy-students/)

* **Redis** can provide a much more durable and powerful cache layer to the prototype distributed system, however, you should take note of one keyword in the requirement: **multithreaded**. In terms of commands execution, **Redis is mostly a single-threaded server**. It is not designed to benefit from multiple CPU cores unlike Memcached, however, you can launch several Redis instances to scale out on several cores if needed. **Memcached** is a more suitable choice since the scenario specifies that the system will run large nodes with multiple cores or threads which Memcached can adequately provide.

* You can choose Memcached over Redis if you have the following requirements:
    1. You need the simplest model possible.
    2. You need to run large nodes with multiple cores or threads.
    3. You need the ability to scale out and in, adding and removing nodes as demand on your system increases and decreases.
    4. You need to cache objects, such as a database.

* The two main components of **Amazon Cognito** are user pools and identity pools. **User pools** are user directories that provide sign-up and sign-in options for your app users. **Identity pools** enable you to grant your users access to other AWS services. You can use identity pools and user pools separately or together.

* Amazon Cognito identity pools provide temporary AWS credentials for users who are guests (unauthenticated) and for users who have been authenticated and received a token. An identity pool is a store of user identity data specific to your account.Amazon Cognito identity pools enable you to create unique identities and assign permissions for users. Your identity pool can include:
    1. Users in an Amazon Cognito user pool

    2. Users who authenticate with external identity providers such as Facebook, Google, or a SAML-based identity provider

    3. Users authenticated via your own existing authentication process

* Databases employ locking mechanisms to ensure that data is always updated to the latest version and is concurrent. There are multiple types of locking strategies that benefit different use cases. Some of these are:

    1. Optimistic Locking: Optimistic locking is a strategy to ensure that the client-side item that you are updating (or deleting) is the same as the item in DynamoDB. If you use this strategy, then your database writes are protected from being overwritten by the writes of others — and vice-verse. 

    2. Pessimistic Locking

    3.  Overly Optimistic Locking
    


## Other Important Link

* [Amazon Simple Workflow (SWF) vs AWS Step Functions vs Amazon SQS](https://tutorialsdojo.com/amazon-simple-workflow-swf-vs-aws-step-functions-vs-amazon-sqs/)

* [Comparison of AWS Services Cheat Sheets](https://tutorialsdojo.com/comparison-of-aws-services-for-udemy-students/)

* [Kinesis Scaling, Resharding and Parallel Processing](https://tutorialsdojo.com/kinesis-scaling-resharding-and-parallel-processing/)

* [Redis (cluster mode enabled vs disabled) vs Memcached](https://tutorialsdojo.com/redis-cluster-mode-enabled-vs-disabled-vs-memcached/)

* [AWS Lambda Integration with Amazon DynamoDB Streams](https://tutorialsdojo.com/aws-lambda-integration-with-amazon-dynamodb-streams/)

* [RDS vs DynamoDB](https://tutorialsdojo.com/amazon-rds-vs-dynamodb/)

* [DynamoDB Scan vs Query](https://tutorialsdojo.com/dynamodb-scan-vs-query/)

* [Elastic Beanstalk vs CloudFormation vs  OpsWorks vs CodeDeploy](https://tutorialsdojo.com/elastic-beanstalk-vs-cloudformation-vs-opsworks-vs-codedeploy/)


## Redis vs Memcached

* You can choose Memcached over Redis if you have the following requirements:

    1. You need the simplest model possible.
    2. You need to run large nodes with multiple cores or threads.
    3. You need the ability to scale out and in, adding and removing nodes as demand on your system increases and decreases.
    3. You need to cache objects, such as a database.