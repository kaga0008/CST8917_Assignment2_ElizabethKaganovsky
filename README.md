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
| Triggers              | HTTP, Event Grid, Blob Storage, Service Bus, Cosmos DB [1]    | HTTP, S3, DynamoDB, API Gateway, Kinesis, SNS, CloudWatch [1]                 | HTTP, Pub/Sub, Cloud Storage, Firestore, [2]              |
| Bindings              | Available                                                           | Not available                                                                       | Not available                                                   |
| Pricing Model         | 1M requests/400k GB/sec free, then pay per execution [3]      | 1M requests/400k GB/sec free, then pay per execution duration + requests [4] | 2M requests/180k GB/sec free, they pay per execution [5] |
| Languages/Runtimes    | Python, .NET, Node.js (JS), Java, C#, F#, PowerShell (Lacks Go, Ruby) [2] | Python, .NET, Node.js (JS), Java, Go, C#, Ruby [2]                                        | Python, .NET, Node.js (JS), Java, Go, Ruby, PHP                 |
| Max Execution Timeout | 10 mins (Consumption), Unlimited (Premium) [6]                | 9 mins [6]                                                                    | 9 mins (1st gen), 60 mins (2nd gen) [6]                   |                                                        | 9 Minutes [3]                                             |

|            | Azure Functions                                                       | AWS Lambda                                                                               | GCP Cloud Run Functions                                                                        |
|------------|-----------------------------------------------------------------------|------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|
| Strengths  | Strong MS Ecosystem integration (particularly logic apps) [7] | Large trigger library, granular scaling capabilities, custom runtime support [7] | High portability [7]                                                                   |
| Weaknesses | Cold start issues for budget options                                  | Lacks bindings                                                                           | Lacks bindings, higher barrier to entry (architecturally dissimilar to Azure Functions/Lambda) |

Notably, AWS Lambda and GCP Cloud Run Functions lack the complex bindings afforded by Azure Functions, instead using JSON objects as inputs and outputs. The type of event that triggers the function determines the schema of the associated objects [2]. Due to the similarities between all options, choice often depends on the existing ecosystem the service will be intergrated into. 

### Durable Functions
Each major CSP provides its own answer to the issue of statelessness in serverless workflows. AWS notably provides two options, AWS Step Functions and AWS Lambda Durable Functions that both can be used for orchestrating serverless workflows and persisting data between executions of steps in a workflow. However, AWS's guide on picking between the two options suggests that the new Lambda Durable Functions are more in-line with the functionality presented by similar services in Azure [8]. GCP similarly provides two roughly analagous services, Cloud Workflows and Cloud Run Functions, but Microsoft identifies Cloud Run Functions as the closer comparison [9], though Cloud Workflows is still necessary to enforce statefulness as Cloud Run Functions does not naturally boast the same durability featues as Azure and AWS.

|                                  | Azure Durable Functions                                        | AWS Durable Lambda Functions                                                        | GCP Cloud Run Functions + Cloud Workflows                                 |
|----------------------------------|----------------------------------------------------------------|-------------------------------------------------------------------------------------|---------------------------------------------------------------------------|
| Orchestration                    | Managed via Durable Task Framework, runs within Function App   | Directly within Lambda, no separate service like Step Functions required            | Cloud Spanner                                                             |
| Execution model/state management | Azure Storage (queue, table, blob)                             | Durable Execution SDK [10]                                                 | Cloud Workflows for state management, Cloud Spanner for storage [11] |
| Monitoring                       | Azure Monitor                                                  | CloudWatch                                                                          | Cloud Monitoring                                                          |
| Pricing Model                    | 1M requests/400k GB/sec free, then pay per execution [3] | 1M requests/400k GB/sec free, then pay per execution duration + requests [4] | 2M requests/180k GB/sec free, they pay per execution [5]           |

|            | Azure Durable Functions                                             | AWS Durable Lambda Functions                                   | GCP Cloud Run Functions + Cloud Workflows                               |
|------------|---------------------------------------------------------------------|----------------------------------------------------------------|-------------------------------------------------------------------------|
| Strengths  | Mature durable framework, stateful persistence is easy to implement | Easy integration with other AWS services inherited from Lambda | Cloud Workflows provides a visual display for no/low code orchestration |
| Weaknesses | Strong lock-in due to lack of analogous services                    | Immature service, high likelihood of bugs/poor documentation   | Requires two services to manage stateful workflows                      |

### Logic Apps
When it comes to orchestration tools, services differ in capabilities between each CSP. Notable, GCP offers two roughly analagous services, Application INtegration and Cloud Workflows, oriented to business types and software developers respectively, and combined give abilities similar to those afforded by Logic Apps and Step Functions [11].

|                  | Azure Logic Apps                                            | AWS Step Functions                     | GCP Application Integration                                      | GCP Cloud Workflows                               |
|------------------|-------------------------------------------------------------|----------------------------------------|------------------------------------------------------------------|---------------------------------------------------|
| User Experience  | Drag-and-drop designer                                      | JSON-based                             | Low code/No code                                                 | Developer-centric, YAML/JSON definitions          |
| Connectors       | Extremely extensive, MS and third-party SaaS                | Extensive, but primarily AWS services  | Extensive, but primarily GCP services                            | Extensive, but primarily GCP services             |
| Pricing model    | Consumption (per-action + connector usage) or Standard plan | Per-state transition or duration-based | Connection hours, volume of data processed, executions [12] | Execution steps and external HTTP calls [12] |
| Primary use case | Business process automation and workflow orchestration      | AWS Service orchestration              | GCP Service orchestration                                        | GCP service orchestration                         |
| Monitoring       | Azure Monitor                                               | CloudWatch                             | Cloud Monitoring                                                 | Cloud Monitoring                                  |

To note is that AWS Step Functions deos not provide the same user-friendly interface and requires knowledge of either JSON or the proprietary Amazon States Language (ASL), making them an unfriendly choice for low-code/no-code environments.

|            | Azure Logic Apps                                                                                               | AWS Step Functions                                                               | GCP Application Integration              | GCP Cloud Workflows                     |
|------------|----------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|------------------------------------------|-----------------------------------------|
| Strengths  | Massive family of connectors                                                                                   | Strong native AWS integration                                                    | Friendly to non-developers (low/no-code) | Strong integration with Google services |
| Weaknesses | Additional fees for certain connectors [13], high lock in due to lack of support for other connectors in other CSPs | Requires knowledge of JSON/Proprietary ASL, limited usability for non-developers | Limited non-Google ecosystem connectors  | Limited non-Google ecosystem connectors |

### Service Bus
Similar to the above scenarios, some CSPs do not provide perfect matches. AWS requires Simple Queueing Service for message queueing and Simple Notification Service for pub/sub behaviour.

|                     | Azure Service Bus                                   | AWS SQS + SNS                                                   | GCP Cloud Pub/Sub                                     |
|---------------------|-----------------------------------------------------|-----------------------------------------------------------------|-------------------------------------------------------|
| Messaging Model     | Enterprise Message Broker (Queues & Pub/Sub Topics) | Message Queue (Point-to-point, for fan-out use SNS) [14] | Global Pub/Sub (Topic-Subscription model) [14] |
| Max Payload         | 1 MB (Standard/Premium)                             | 256 KB (up to 2GB with S3 SDK) [14]                      | 10 MB [14]                                     |
| Protocols           | AMQP 1.0, HTTP/REST                                 | HTTP/REST [14]                                           | gRPC, HTTP/REST [14]                           |
| Delivery Guarantees | At least once (Exactly once with Sessions)          | At least once (Standard), exactly once (FIFO) [14]       | At least once [14]                             |
| Monitoring          | Azure Monitor                                       | CloudWatch                                                      | Cloud Monitor                                         |

|            | Azure Service Bus             | AWS SQS + SNS                                       | GCP Cloud Pub/Sub                                          |
|------------|-------------------------------|-----------------------------------------------------|------------------------------------------------------------|
| Strengths  | Strong enterprise integration | Simple setup, low management overhead               | Strong integration with BigQuery and Google Cloud Dataflow |
| Weaknesses | Strong Azure lock-in          | Two services required for a full suite of features  | Tightly coupled to GCP ecosystem                           |

### Event Grid
|       | Azure Event Grid                     | AWS EventBridge                                   | GCP EventArc                                                                                |
|-------|--------------------------------------|---------------------------------------------------|---------------------------------------------------------------------------------------------|
| Query | Use Functions to query [15] | Query language for filtering events [16] | APIs for message publishing, message subscription, and message acknowledgment [16] |

oh my god i'm so tired of these I'M SORRY

- Azure Event Grid
- AWS EventBridge
- GCP Eventarc

### Event Hub
- Azure Event Hub
- AWS Kinesis Data Streams
- GCP Cloud Pub/Sub

## References
[1] ggailey777, “Triggers and Bindings in Azure Functions,” Microsoft.com, Feb. 26, 2026. https://learn.microsoft.com/en-us/azure/azure-functions/functions-triggers-bindings (accessed Apr. 07, 2026).
‌

[2] “AWS Lambda vs. Azure Functions: 10 Major Differences,” IOD, Oct. 20, 2019. https://iamondemand.com/blog/aws-lambda-vs-azure-functions-ten-major-differences/
‌

[3] “Pricing - Functions | Microsoft Azure,” azure.microsoft.com. https://azure.microsoft.com/en-us/pricing/details/functions/
‌

[4]AWS, “AWS Lambda – Pricing,” Amazon Web Services, Inc., 2024. https://aws.amazon.com/lambda/pricing/
‌

[5] “Pricing | Cloud Run,” Google Cloud. https://cloud.google.com/run/pricing
‌

[6] “AWS Lambda vs Azure Functions vs Google Cloud Functions | Hokstad Consulting,” Hokstadconsulting.com, 2025. https://hokstadconsulting.com/blog/aws-lambda-vs-azure-functions-vs-google-cloud-functions (accessed Apr. 07, 2026).


[7] V. Krishnakumar, “AWS Lambda vs Azure Functions vs Google Cloud Functions: A Detailed Serverless Comparison,” CloudOptimo, Jun. 23, 2025. https://www.cloudoptimo.com/blog/aws-lambda-vs-azure-functions-vs-google-cloud-functions-a-detailed-serverless-comparison/
‌

[8] “Durable functions or Step Functions - AWS Lambda,” Amazon.com, 2026. https://docs.aws.amazon.com/lambda/latest/dg/durable-step-functions.html (accessed Apr. 07, 2026).
‌

[9] EdPrice-MSFT, “Google Cloud to Azure services comparison - Azure Architecture Center,” learn.microsoft.com. https://learn.microsoft.com/en-us/azure/architecture/gcp-professional/services


[10] “Durable execution SDK - AWS Lambda,” Amazon.com, 2026. https://docs.aws.amazon.com/lambda/latest/dg/durable-execution-sdk.html (accessed Apr. 07, 2026).
‌‌

[11] "Choose Workflows or Application Integration‌," GCP, 2025. https://docs.cloud.google.com/workflows/docs/choose-app-integ-or-workflows 


[12] “Cloud Run: Container to production in seconds,” Google Cloud. https://cloud.google.com/run
‌

[13] ecfan, “Usage metering, billing, and pricing - Azure Logic Apps,” Microsoft.com, Sep. 08, 2024. https://learn.microsoft.com/en-us/azure/logic-apps/logic-apps-pricing


[14] “Amazon SNS vs Google Cloud Pub/Sub: which should you choose in 2026?,” Ably Realtime, 2025. https://ably.com/compare/amazon-sns-vs-google-pub-sub (accessed Apr. 07, 2026).


[15] “azure event grid vs aws eventbridge: Which Tool is Better for Your Next Project?,” Projectpro.io, 2022. https://www.projectpro.io/compare/azure-event-grid-vs-aws-eventbridge (accessed Apr. 07, 2026).
‌

[16] “google cloud pub sub vs aws eventbridge: Which Tool is Better for Your Next Project?,” Projectpro.io, 2022. https://www.projectpro.io/compare/google-cloud-pub-sub-vs-aws-eventbridge (accessed Apr. 09, 2026).
‌