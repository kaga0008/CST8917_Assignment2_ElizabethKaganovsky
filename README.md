# CST8917 Assignment 2 – Serverless Service Alternatives Report
## Elizabeth Kaganovsky (040956095) - April 8, 2026

| Azure Offering    | AWS Offering                                                               | GCP Offering            |
|-------------------|----------------------------------------------------------------------------|-------------------------|
| Functions         | Lambda                                                                     | Cloud Run Functions     |
| Durable Functions | Lambda Durable Functions                                                   | Cloud Run Functions + Cloud Workflows         |
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
Each major CSP provides its own answer to the issue of statelessness in serverless workflows. AWS notably provides two options, AWS Step Functions and AWS Lambda Durable Functions that both can be used for orchestrating serverless workflows and persisting data between executions of steps in a workflow. However, AWS's guide on picking between the two options suggests that Lambda Durable Functions are more in-line with the functionality presented by similar services in Azure [awsdurfunc]. GCP similarly provides two roughly analagous services, Cloud Workflows and Cloud Run Functions, but Microsoft identifies Cloud Run Functions as the closer comparison [gcpcloudfunc], though Cloud Workflows is still necessary to enforce statefulness as Cloud Run Functions does not naturally boast the same durability featues as Azure and AWS.

|                                  | Azure Durable Functions                                        | AWS Durable Lambda Functions                                                        | GCP Cloud Run Functions + Cloud Workflows                                 |
|----------------------------------|----------------------------------------------------------------|-------------------------------------------------------------------------------------|---------------------------------------------------------------------------|
| Orchestration                    | Managed via Durable Task Framework, runs within Function App   | Directly within Lambda, no separate service like Step Functions required            | Cloud Spanner                                                             |
| Execution model/state management | Azure Storage (queue, table, blob)                             | Durable Execution SDK [awsdurexsdk]                                                 | Cloud Workflows for state management, Cloud Spanner for storage [gcpspan] |
| Monitoring                       | Azure Monitor                                                  | CloudWatch                                                                          | Cloud Monitoring                                                          |
| Pricing Model                    | 1M requests/400k GB/sec free, then pay per execution [azprice] | 1M requests/400k GB/sec free, then pay per execution duration + requests [awsprice] | 2M requests/180k GB/sec free, they pay per execution [gcpPrice]           |

|            | Azure Durable Functions                                             | AWS Durable Lambda Functions                                   | GCP Cloud Run Functions + Cloud Workflows                               |
|------------|---------------------------------------------------------------------|----------------------------------------------------------------|-------------------------------------------------------------------------|
| Strengths  | Mature durable framework, stateful persistence is easy to implement | Easy integration with other AWS services inherited from Lambda | Cloud Workflows provides a visual display for no/low code orchestration |
| Weaknesses | Strong lock-in due to lack of analogous services                    | Immature service, high likelihood of bugs/poor documentation   | Requires two services to manage stateful workflows                      |

### Logic Apps
When it comes to orchestration tools, services differ in capabilities between each CSP. Notable, GCP offers two roughly analagous services, Application INtegration and Cloud Workflows, oriented to business types and software developers respectively, and combined give abilities similar to those afforded by Logic Apps and Step Functions.

|                  | Azure Logic Apps                                            | AWS Step Functions                     | GCP Application Integration                                      | GCP Cloud Workflows                               |
|------------------|-------------------------------------------------------------|----------------------------------------|------------------------------------------------------------------|---------------------------------------------------|
| User Experience  | Drag-and-drop designer                                      | JSON-based                             | Low code/No code                                                 | Developer-centric, YAML/JSON definitions          |
| Connectors       | Extremely extensive, MS and third-party SaaS                | Extensive, but primarily AWS services  | Extensive, but primarily GCP services                            | Extensive, but primarily GCP services             |
| Pricing model    | Consumption (per-action + connector usage) or Standard plan | Per-state transition or duration-based | Connection hours, volume of data processed, executions [gcpaicw] | Execution steps and external HTTP calls [gcpaicw] |
| Primary use case | Business process automation and workflow orchestration      | AWS Service orchestration              | GCP Service orchestration                                        | GCP service orchestration                         |
| Monitoring       | Azure Monitor                                               | CloudWatch                             | Cloud Monitoring                                                 | Cloud Monitoring                                  |

To note is that AWS Step Functions deos not provide the same user-friendly interface and requires knowledge of either JSON or the proprietary Amazon States Language (ASL), making them an unfriendly choice for low-code/no-code environments.

|            | Azure Logic Apps                                                                                               | AWS Step Functions                                                               | GCP Application Integration              | GCP Cloud Workflows                     |
|------------|----------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|------------------------------------------|-----------------------------------------|
| Strengths  | Massive family of connectors                                                                                   | Strong native AWS integration                                                    | Friendly to non-developers (low/no-code) | Strong integration with Google services |
| Weaknesses | Additional fees for certain connectors [azconnfee], high lock in due to lack of support for other connectors in other CSPs | Requires knowledge of JSON/Proprietary ASL, limited usability for non-developers | Limited non-Google ecosystem connectors  | Limited non-Google ecosystem connectors |



### Service Bus
- Azure Service Bus
- AWS SQS
- AWS SNS
- GCP Cloud Pub/Sub

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
[gcpawcw] https://docs.cloud.google.com/workflows/docs/choose-app-integ-or-workflows
[azconnfee] https://learn.microsoft.com/en-us/azure/logic-apps/logic-apps-pricing
[gcpcloudfunc] https://learn.microsoft.com/en-us/azure/architecture/gcp-professional/services
[awsdurfunc] https://docs.aws.amazon.com/lambda/latest/dg/durable-step-functions.html 
[awsdurexsdk] https://docs.aws.amazon.com/lambda/latest/dg/durable-execution-sdk.html
[gcpspan] https://cloud.google.com/run 

Other
https://multishoring.com/blog/azure-logic-apps-vs-aws-step-functions-which-fits-your-integration-needs/