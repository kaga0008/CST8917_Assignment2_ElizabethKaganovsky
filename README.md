# CST8917 Assignment 2 – Serverless Service Alternatives Report
## Elizabeth Kaganovsky (040956095) - April 8, 2026

| Azure Offering    | AWS Offering                                                               | GCP Offering            |
|-------------------|----------------------------------------------------------------------------|-------------------------|
| Functions         | Lambda                                                                     | Cloud Run Functions     |
| Durable Functions | Lambda Durable Functions, Step Functions                                   | Cloud Workflows         |
| Logic Apps        | Step Functions (Closest match)                                             | Application Integration |
| Service Bus       | Simple Queue Service (for queues) Simple Notification Service (for topics) | Cloud Pub/Sub           |
| Event Grid        | EventBridge                                                                | Eventarc                |
| Event Hub         | Kinesis Data Streams                                                       | Cloud Pub/Sub           |

### Serverless Functions
The offerings provided by the three major CSPs are functionally quite similar, and choice significantly depends on the existing cloud environment for which the functions will be integrated into. Differences are detailed in the table below:

|                       | Azure Functions                                                     | AWS Lambda                                                                          | GCP Cloud Run Functions                                         |
|-----------------------|---------------------------------------------------------------------|-------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Triggers              | HTTP, Event Grid, Blob Storage, Service Bus, Cosmos DB [timeout]    | HTTP, S3, DynamoDB, API Gateway, Kinesis, SNS, CloudWatch [timeout]                 | HTTP, Pub/Sub, Cloud Storage, Firestore, [timeout]              |
| Bindings              | Available                                                           | Not available                                                                       | Not available                                                   |
| Pricing Model         | 1M requests/400k GB/sec free, then pay per execution [azprice]      | 1M requests/400k GB/sec free, then pay per execution duration + requests [awsprice] | 2M requests/180k GB/sec free, they pay per execution [gcpPrice] |
| Languages/Runtimes    | Python, .NET, Node.js (JS), Java, C#, F#, PowerShell (Lacks Go, Ruby) [2] | Python, .NET, Node.js (JS), Java, Go, C#, Ruby [2]                                        | Python, .NET, Node.js (JS), Java, Go, Ruby, PHP                 |
| Max Execution Timeout | 10 mins (Consumption), Unlimited (Premium) [hokstad]                | 9 mins [hokstad]                                                                    | 9 mins (1st gen), 60 mins (2nd gen) [hokstad]                   |                                                        | 9 Minutes [timeout]                                             |

|            | Azure Functions                                                       | AWS Lambda                                                                               | GCP Cloud Run Functions                                                                        |
|------------|-----------------------------------------------------------------------|------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|
| Strengths  | Strong MS Ecosystem integration (particularly logic apps) [funcstrwk] | Large trigger library, granular scaling capabilities, custom runtime support [funcstrwk] | High portability [funcstrwk]                                                                   |
| Weaknesses | Cold start issues for budget options                                  | Lacks bindings                                                                           | Lacks bindings, higher barrier to entry (architecturally dissimilar to Azure Functions/Lambda) |

Notably, AWS Lambda and GCP Cloud Run Functions lack the complex bindings afforded by Azure Functions, instead using JSON objects as inputs and outputs. The type of event that triggers the function determines the schema of the associated objects [2]. Due to the similarities between all options, choice often depends on the existing ecosystem the service will be intergrated into. 

### Durable Functions
Each major CSP provides its own answer to the issue of statelessness in serverless workflows

- Azure Durable Functions
- AWS Lambda Durable Functions
- AWS Step Functions
- GCP Cloud Workflows

### Logic Apps
When it comes to orchestration tools, services differ in capabilities between each CSP. Notable, GCP offers two roughly analagous services, Application INtegration and Cloud Workflows, oriented to business types and software developers respectively, and combined give abilities similar to those afforded by Logic Apps and Step Functions.

- Azure Logic Apps
- AWS Step Functions
- GCP Application Integration
- GCP Cloud Workflows

### Service Bus
- Azure Service Bus
- AWS SQS
- AWS SNS
- GCP Clod Pub/Sub

### Event Grid
- Azure Event Grid
- AWS EventBridge
- GCP Eventarc

### Event Hub
- Azure Event Hub
- AWS Kinesis Data Streams
- GCP Cloud Pub/Sub

[1] https://learn.microsoft.com/en-us/azure/azure-functions/functions-triggers-bindings?tabs=isolated-process%2Cnode-v4%2Cpython-v2&pivots=programming-language-python

[2] https://iamondemand.com/blog/aws-lambda-vs-azure-functions-ten-major-differences/

[azPrice] https://azure.microsoft.com/en-us/pricing/details/functions/
[awsPrice] https://aws.amazon.com/lambda/pricing/
[gcpPrice] https://cloud.google.com/run/pricing
[timeout] https://sedai.io/blog/lambda-vs-azure-vs-google-cloud
[hokstad] https://hokstadconsulting.com/blog/aws-lambda-vs-azure-functions-vs-google-cloud-functions
[funcstrwk] https://www.cloudoptimo.com/blog/aws-lambda-vs-azure-functions-vs-google-cloud-functions-a-detailed-serverless-comparison/