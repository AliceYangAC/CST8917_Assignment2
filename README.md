# CST8917 Assignment 2: Serverless Service Alternatives Report

## Azure Functions vs. AWS Lambda vs. GCP Cloud Functions

### Overview

Serverless compute is the foundational element of modern event-driven infrastructure, abstracting server management and scaling elastically on demand [1].

- Azure Functions: Microsoft's serverless compute service, tightly integrated with the Azure ecosystem and Microsoft 365. Ideal for organizations with existing Microsoft licensing [8].
- AWS Lambda: Amazon's serverless offering, supporting the broadest range of triggers across the AWS service catalog and optimized for microservices architectures [1].
- GCP Cloud Functions: Google's event-driven compute platform, purpose-built for real-time data processing and ML pipeline triggers [10].

### Core Features

| Feature            | Azure Functions                                                                | AWS Lambda                                                 | GCP Cloud Functions                                         |
| ------------------ | ------------------------------------------------------------------------------ | ---------------------------------------------------------- | ----------------------------------------------------------- |
| Primary Use Case   | Event-driven apps; MS ecosystem integration [8]                                | Microservices; broad AWS stack [1]                         | Real-time data; ML triggers [10]                            |
| Supported Triggers | HTTP, Blob Storage, Timer, Cosmos DB, Service Bus, Event Grid [8]              | HTTP (API Gateway), S3, DynamoDB, SQS, SNS [1]             | HTTP, Pub/Sub, Cloud Storage, Firestore [10]                |
| Bindings           | Input/output bindings to 20+ Azure services (declarative, no SDK required) [8] | Event source mappings; manual SDK integration [1]          | Triggers only; integrations require explicit SDK calls [10] |
| Runtime Support    | .NET, Python, Java, Node.js, PowerShell, C# [8]                                | Node.js, Python, Java, Go, Ruby, .NET, custom runtimes [1] | Node.js, Python, Go, Java, Ruby, PHP [10]                   |
| Max Execution Time | 10 mins (230s HTTP limit) (Consumption); unlimited (Premium/Dedicated) [8]     | 15 minutes [1]                                             | 60 minutes (2nd gen) [7]                                    |

### Integration Options

- Azure Functions integrates natively with virtually all Azure services via declarative bindings, and connects to on-premises systems through Azure Hybrid Connections. CI/CD is supported via Azure DevOps and GitHub Actions [8].
- AWS Lambda connects to the full AWS ecosystem (S3, DynamoDB, SQS, API Gateway) and supports CI/CD through AWS CodePipeline, GitHub Actions, and the Serverless Framework [1].
- GCP Cloud Functions integrates with Firebase, BigQuery, and Cloud Run. GCP's Cloud Build provides CI/CD capability, and Eventarc enables event-based trigger routing [10].

### Monitoring & Observability

- Azure Functions: Azure Monitor, Application Insights (distributed tracing, live metrics, failure analysis), and Log Analytics Workspaces [8].
- AWS Lambda: AWS CloudWatch (logs, metrics, alarms), AWS X-Ray (distributed tracing), and Lambda Insights for enhanced runtime telemetry [1].
- GCP Cloud Functions: Google Cloud Monitoring, Cloud Logging, and Cloud Trace. Integrates with Cloud Debugger for production debugging [10].

### Pricing Model

| Factor         | Azure Functions                                                                   | AWS Lambda                                                      | GCP Cloud Functions                                                              |
| -------------- | --------------------------------------------------------------------------------- | --------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| Billing Unit   | Per-millisecond execution [6]                                                     | 1ms granularity [6]                                             | Per-millisecond via Cloud Run [7]                                                |
| Free Tier      | 1M requests/month, 400,000 GB-s [6]                                               | 1M requests/month, 400,000 GB-s [6]                             | 2M invocations/month [7]                                                         |
| Key Cost Lever | Azure Hybrid Benefit (up to 40% TCO reduction on existing Microsoft licenses) [6] | Graviton4 ARM instances (25% price-performance improvement) [7] | Custom VM types eliminate over-provisioning; $300 free credit for new users [10] |

### Strengths & Weaknesses

|            | Azure Functions                                                                                                           | AWS Lambda                                                                                  | GCP Cloud Functions                                                                         |
| ---------- | ------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| Strengths  | Best-in-class MS ecosystem bindings; Hybrid Benefit ROI for enterprises; Durable Functions for stateful orchestration [8] | Largest trigger ecosystem; best raw price-performance with Graviton4; massive community [1] | Custom machine types; tight BigQuery/ML integration; cost-effective for startups [10]       |
| Weaknesses | Cold start times can be significant on Consumption plan; complex billing for non-MS workloads [8]                         | No native stateful orchestration (requires Step Functions); more complex trigger wiring [1] | Smaller connector ecosystem; less enterprise adoption; bindings less mature than Azure [10] |

### Narrative Analysis

While AWS Lambda's Graviton4 ARM-based instances offer superior raw price-performance, the practical advantage for Windows-heavy or Microsoft-licensed enterprises lies in the Azure Hybrid Benefit. The ability to repurpose existing Windows Server and SQL Server licenses can yield a total cost of ownership reduction of up to 40%, a saving that frequently outweighs the raw compute gains of competing platforms [6]. Conversely, GCP's ability to specify exact vCPU and RAM counts via custom machine types offers a compelling cost optimization tool for startups seeking to eliminate over-provisioning [10].

---

## Durable Functions vs. AWS Step Functions vs. GCP Workflows

### Overview

Without robust orchestration, distributed architectures risk becoming "distributed monoliths", tightly coupled systems with no state visibility that fail silently [8]. Durable Functions and its equivalents solve this by managing retries, checkpointing, and long-running workflow state.

- Azure Durable Functions: A code-centric extension of Azure Functions for defining stateful workflows in C#, Python, JavaScript, or Java. Supports chaining, fan-out/fan-in, and human interaction patterns [8].
- AWS Step Functions: A visual and code-configurable state machine service using Amazon States Language (ASL). Two modes: Express (short-lived, high-volume) and Standard (long-running, exactly-once) [1].
- GCP Workflows: A serverless orchestration service for sequencing Google Cloud and external API calls, defined in YAML or JSON. Tightly integrated with Cloud Run and GCP services [1].

### Core Features

| Feature               | Azure Durable Functions                                   | AWS Step Functions                                 | GCP Workflows                  |
| --------------------- | --------------------------------------------------------- | -------------------------------------------------- | ------------------------------ |
| Definition Style      | Code (C#, Python, JS, Java) [8]                           | JSON/YAML state machine (ASL) + SDK [1]            | YAML/JSON syntax [1]           |
| Chaining              | Yes: `yield context.call_activity()` [8]                  | Yes: Sequential state transitions [1]              | Yes: Sequential step calls [1] |
| Fan-out / Fan-in      | Yes: `Task.WhenAll()` with `call_activity_with_retry` [8] | Yes: Map state (parallel iterations) [1]           | Yes: Parallel branches [1]     |
| Human Approval / Wait | Yes: External events + `wait_for_external_event()` [8]    | Yes: Wait for task token (`.waitForTaskToken`) [1] | Yes: Callback endpoints [1]    |
| State Persistence     | Azure Storage (queues + tables) or Netherite backend [8]  | Managed by AWS (Standard: 90-day history) [1]      | Managed by GCP [1]             |
| Long-Running Support  | Unlimited (timer-based replay) [8]                        | Standard: up to 1 year; Express: 5 minutes [1]     | Up to 1 year [1]               |

### Integration Options

- Durable Functions integrates with all Azure services through activity functions, and connects to external systems via HTTP calls or Service Bus. Deployable via Azure DevOps, GitHub Actions, or the Azure Functions Core Tools [8].
- Step Functions integrates natively with 220+ AWS services via optimized integrations (direct SDK calls without Lambda wrappers). Supports EventBridge for event-triggered workflows [1].
- GCP Workflows integrates with Cloud Run, Cloud Functions, BigQuery, and external HTTP APIs. Triggerable via Cloud Scheduler, Eventarc, and Pub/Sub [1].

### Monitoring & Observability

- Durable Functions: Application Insights automatically traces orchestration instance history, replay events, and activity execution. The Durable Task Framework exposes custom events [8].
- Step Functions: AWS CloudWatch provides execution metrics and logs. Step Functions Visual Console shows real-time execution graphs and step-by-step state transitions [1].
- GCP Workflows: Cloud Logging captures all workflow execution logs. Cloud Monitoring surfaces execution metrics and can trigger alerts on failures [1].

### Pricing Model

|           | Azure Durable Functions                                             | AWS Step Functions                                                             | GCP Workflows                                  |
| --------- | ------------------------------------------------------------------- | ------------------------------------------------------------------------------ | ---------------------------------------------- |
| Model     | Included in Azure Functions consumption billing + storage costs [8] | Standard: per state transition; Express: per execution duration + requests [1] | Per internal step + per external HTTP call [1] |
| Free Tier | Inherits Functions free tier [8]                                    | Standard: 4,000 state transitions/month; Express: 1M requests/month [1]        | 5,000 internal steps/month [1]                 |

### Strengths & Weaknesses

|            | Durable Functions                                                                                                                                      | Step Functions                                                                        | GCP Workflows                                                                       |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| Strengths  | Full programming language expressiveness; no DSL to learn; replay-based durability is transparent [8]                                                  | 220+ native AWS integrations; excellent visual debugger; no Lambda wrapper needed [1] | Simple YAML syntax; tight GCP integration; cost-effective for API orchestration [1] |
| Weaknesses | Replay debugging can be confusing; storage backend adds latency; less visual tooling. Requires strict deterministic code to prevent replay errors. [8] | ASL JSON becomes verbose for complex workflows; Express mode lacks exactly-once [1]   | Less expressive than code-based alternatives; limited community resources [1]       |

### Narrative Analysis

Durable Functions provides tools for complex orchestration patterns, including chaining, fan-out/fan-in, and actor-model workflows, entirely in familiar programming languages, without requiring developers to learn a separate DSL [8]. These tools provide the critical reliability layer by managing retries and checkpoints, ensuring that complex microservices chains remain resilient and observable [8]. AWS Step Functions offers a compelling alternative for teams that prefer visual workflow design and need broad AWS service integrations without writing glue Lambda functions. GCP Workflows is the simplest of the three but trades expressiveness for ease of use.

---

## Azure Logic Apps vs. AWS Step Functions (Standard) vs. GCP Workflows

### Overview

Logic Apps and its equivalents target business process automation, connecting SaaS applications, on-premises systems, and cloud services via a visual, low-code interface.

- Azure Logic Apps: A visual integration platform with 200+ pre-built connectors to SaaS and enterprise systems. The gold standard for business process automation in the Azure ecosystem [8].
- AWS Step Functions (Standard Workflows): When paired with EventBridge and pre-built integrations, Step Functions serves as AWS's closest equivalent to Logic Apps for business workflow automation [1].
- GCP Workflows: GCP does not have a direct low-code Logic Apps equivalent; Workflows with Application Integration (formerly Apigee Integration) is the closest match [1].

### Core Features

| Feature                  | Azure Logic Apps                                            | AWS Step Functions + EventBridge        | GCP Application Integration                  |
| ------------------------ | ----------------------------------------------------------- | --------------------------------------- | -------------------------------------------- |
| Design Interface         | Visual drag-and-drop designer [8]                           | Visual state machine editor [1]         | Visual designer (Apigee-based) [1]           |
| Pre-built Connectors     | 200+ (Office 365, SAP, Salesforce, ServiceNow) [8]          | 220+ AWS service integrations [1]       | Google Workspace, Salesforce, ServiceNow [1] |
| On-Premises Connectivity | On-premises data gateway [8]                                | Limited (via VPN or Direct Connect) [1] | Hybrid connectivity via Apigee [1]           |
| Trigger Types            | HTTP, recurrence (schedule), event-based, message-based [8] | EventBridge rules, API Gateway, SQS [1] | HTTP, Cloud Scheduler, Pub/Sub [1]           |
| Low-Code Capability      | High; minimal code required [8]                             | Medium; ASL JSON still required [1]     | Medium; YAML/connector config [1]            |

### Integration Options

- Logic Apps connects to on-premises systems via the on-premises data gateway, integrates with Azure AD for identity, and supports B2B workflows with EDI/AS2 [8].
- Step Functions + EventBridge connects to AWS services natively and to external systems via Lambda or HTTP integrations [1].
- GCP Application Integration connects to Google Workspace natively and to third-party SaaS via pre-built connectors or REST/gRPC calls [1].

### Monitoring & Observability

- Logic Apps: Azure Monitor and the Logic Apps run history viewer provide detailed step-by-step execution logs, input/output inspection, and retry tracking [8].
- Step Functions: CloudWatch dashboards and the Step Functions execution console provide visual state-by-state execution history [1].
- GCP Application Integration: Cloud Logging and built-in execution history in the Application Integration console [1].

### Pricing Model

|          | Logic Apps                                                                      | Step Functions                   | GCP Application Integration                     |
| -------- | ------------------------------------------------------------------------------- | -------------------------------- | ----------------------------------------------- |
| Model    | Consumption (per action/trigger execution) or Standard (fixed hosting plan) [8] | Per state transition [1]         | Per integration execution step [1]              |
| Key Note | Premium connectors (SAP, Salesforce) billed additionally [8]                    | No connector licensing costs [1] | Some connectors require Apigee subscription [1] |

### Strengths & Weaknesses

|            | Logic Apps                                                                                                                                        | Step Functions                                        | GCP Application Integration                                |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- | ---------------------------------------------------------- |
| Strengths  | Widest SaaS connector library; true no-code for business users; native B2B support [8]                                                            | Tight AWS integration; no per-connector licensing [1] | Good for Google Workspace automation [1]                   |
| Weaknesses | Can become expensive at scale with premium connectors; limited versioning/ALM. Vendor lock-in is higher here than in lower code alternatives. [8] | Not truly low-code; requires JSON/ASL knowledge [1]   | Immature vs. Logic Apps; limited non-Google connectors [1] |

### Narrative Analysis

Azure Logic Apps serves as the gold standard for business process automation, providing a visual designer and 200+ connectors to bridge SaaS and on-premises systems [8]. Neither AWS Step Functions nor GCP Application Integration offers an equivalent breadth of pre-built connectors or on-premises integration capability. For organizations that need non-developer users to build and manage workflows, particularly in government or enterprise environments with SAP, Dynamics 365, or Salesforce, Logic Apps remains the clear leader.

---

## Azure Service Bus vs. AWS SQS/SNS vs. GCP Pub/Sub

### Overview

Architects must distinguish between messages (intent-based, high-value, requiring guaranteed delivery) and events (fact-based, high-volume). Service Bus and its equivalents are designed for the former: transactional, business-critical messaging where no message can be lost [11].

- Azure Service Bus: An enterprise message broker with support for queues, topics, and subscriptions. Engineered for transactional integrity and ordered delivery [11].
- AWS SQS/SNS: Amazon's two-part messaging offering: SQS for point-to-point queuing, SNS for fan-out publish/subscribe. Together they approximate Service Bus functionality [1].
- GCP Pub/Sub: Google's globally distributed messaging service, designed for high-throughput event streaming and fan-out delivery at scale [13].

### Core Features

| Capability           | Azure Service Bus                                  | AWS SQS/SNS                                       | GCP Pub/Sub                             |
| -------------------- | -------------------------------------------------- | ------------------------------------------------- | --------------------------------------- |
| Delivery Guarantee   | At-least-once; exactly-once via sessions [11]      | At-least-once (Standard); exactly-once (FIFO) [1] | At-least-once [13]                      |
| Ordering             | Native message ordering via sessions [11]          | FIFO queues with message group IDs [1]            | Ordering keys (per partition) [13]      |
| Dead-Letter Queue    | Yes: Native support [11]                           | Yes: Native support [1]                           | Yes: Native support [13]                |
| Transactions         | Yes: Native atomic operations across entities [11] | No: Limited to single queue scope [1]             | No: No native transaction scope [13]    |
| Message Size Limit   | Standard: 256KB; Premium: 1MB [11]                 | 256KB (S3 for extended payloads) [1]              | 10MB default [13]                       |
| Topics/Subscriptions | Yes: With SQL-like filter rules [11]               | Yes: SNS topics with filter policies [1]          | Yes: Topics with filter attributes [13] |

### Integration Options

- Service Bus integrates with Azure Functions (trigger), Logic Apps (connector), Event Grid (forwarding), and on-premises systems via Service Bus relay. ARM/Bicep and Terraform support for IaC [11].
- SQS/SNS integrates natively with Lambda, Step Functions, EventBridge, and Kinesis. Fully supported in AWS CDK, CloudFormation, and Terraform [1].
- GCP Pub/Sub integrates with Cloud Functions, Dataflow, BigQuery (direct subscription), and Workflows. Supported in Terraform and Deployment Manager [13].

### Monitoring & Observability

- Service Bus: Azure Monitor metrics (message counts, dead-letter counts, active messages), Service Bus Explorer for message inspection, and Application Insights integration for tracing [11].
- SQS/SNS: CloudWatch metrics (ApproximateNumberOfMessagesVisible, NumberOfMessagesSent), CloudWatch Logs for Lambda-SQS triggers, and AWS X-Ray for tracing [1].
- Pub/Sub: Cloud Monitoring (subscription backlog, oldest unacked message age), Cloud Logging for delivery attempts, and Pub/Sub snapshot support for replay [13].

### Pricing Model

|            | Service Bus                                                            | AWS SQS/SNS                                               | GCP Pub/Sub                         |
| ---------- | ---------------------------------------------------------------------- | --------------------------------------------------------- | ----------------------------------- |
| Model      | Standard tier: per operation; Premium tier: fixed messaging units [11] | SQS: pay-per-request; SNS: pay-per-publish + delivery [6] | Pay-per-GB of data [13]             |
| Free Tier  | Standard: 10M operations/month [11]                                    | SQS: 1M requests/month; SNS: 1M publishes/month [6]       | 10 GB/month [13]                    |
| Scale Cost | Premium tier predictable for high-throughput workloads [11]            | Very cost-effective at low-to-mid volume [6]              | Competitive at high throughput [13] |

### Strengths & Weaknesses

|            | Service Bus                                                                                                                 | SQS/SNS                                                                          | GCP Pub/Sub                                                                               |
| ---------- | --------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| Strengths  | Only provider with native cross-entity transactions; best for financial/order workflows; mature enterprise feature set [11] | Simple to use; very cost-effective; FIFO mode for ordering [1]                   | Global delivery; highest default message size (10MB); excellent BigQuery integration [13] |
| Weaknesses | More expensive than SQS at low volume; Premium tier required for VNet isolation [11]                                        | No native cross-queue transactions; SQS and SNS must be combined for pub/sub [1] | No native transaction support; less suited for strict financial integrity scenarios [13]  |

### Narrative Analysis

Service Bus is the preferred choice for financial transactions and order processing [11]. Its support for exactly-once processing via sessions and native dead-lettering ensures that business-critical data is handled with the highest integrity [11]. AWS SQS and SNS together approximate this capability but require separate services to be combined, and cross-queue transactions remain unsupported. GCP Pub/Sub is better classified as a high-throughput event streaming service than a transactional message broker, its lack of native transaction scope makes it less suitable for order-of-record scenarios [13].

---

## Azure Event Grid vs. AWS EventBridge vs. GCP Eventarc

### Overview

Event routers operate on the "Something Happened" paradigm, they distribute facts about state changes to interested subscribers. They form the central nervous system of reactive, event-driven architectures [4].

- Azure Event Grid: Microsoft's event routing service, built around CloudEvents and deeply integrated with Azure resource lifecycle events [12].
- AWS EventBridge: Amazon's event bus service supporting custom events, SaaS partner events, and native AWS service events, with powerful filtering and transformation via EventBridge Pipes [12].
- GCP Eventarc: Google's event routing platform supporting CloudEvents from GCP services and custom sources, with native transformation pipelines in the Advanced tier [4].

### Core Features

| Feature                | Azure Event Grid                          | AWS EventBridge                                         | GCP Eventarc Advanced             |
| ---------------------- | ----------------------------------------- | ------------------------------------------------------- | --------------------------------- |
| Uptime SLA             | 99.99% [12]                               | Not specified [12]                                      | 99.9% [4]                         |
| CloudEvents Support    | Yes: [12]                                 | Yes: [12]                                               | Yes: [4]                          |
| Native Transformation  | No: Requires external Azure Function [11] | Yes: EventBridge Pipes (filter, enrich, route) [15]     | Yes: Transformation pipelines [4] |
| Event Archive & Replay | No: 24-hour retry window only [12]        | Yes: Archive + replay [15]                              | No: 24-hour retry window [4]      |
| Partner Events (SaaS)  | Limited [12]                              | Yes: 35+ SaaS partners (Salesforce, Zendesk, etc.) [15] | Limited [4]                       |
| Event Retention        | 24 hours with retries [12]                | Configurable archive [15]                               | 24 hours with retries [4]         |

### Integration Options

- Event Grid integrates tightly with Azure resource manager events (blob created, resource deployed), Logic Apps, Functions, Service Bus, and Event Hubs as endpoints [12].
- EventBridge integrates with 200+ AWS event sources, SaaS partners, and custom applications. EventBridge Pipes provides point-to-point event routing with filtering between any source and target [15].
- Eventarc integrates with Cloud Run, Cloud Functions, GKE, and Workflows. Direct event routing from 90+ Google Cloud services [4].

### Monitoring & Observability

- Event Grid: Azure Monitor metrics (delivery success/failure counts), Diagnostic Logs for delivery attempts, and Azure Monitor Alerts for dead-letter thresholds [12].
- EventBridge: CloudWatch metrics (invocations, failed invocations, throttled rules), CloudTrail for API-level audit, and EventBridge event archive for forensic replay [15].
- Eventarc: Cloud Logging for delivery logs, Cloud Monitoring for event delivery metrics, and Pub/Sub dead-letter topics for failed events [4].

### Pricing Model

|          | Event Grid                                                   | EventBridge                                                        | GCP Eventarc                                     |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------------ | ------------------------------------------------ |
| Model    | Per operation (first 100K free/month) [12]                   | Per event (custom/partner events); free for AWS service events [3] | Per event delivered [4]                          |
| Key Note | Very cost-effective for Azure-native reactive workloads [12] | EventBridge Pipes billed separately per event processed [3]        | Cross-region delivery billed at higher rates [4] |

### Strengths & Weaknesses

|            | Event Grid                                                                                | EventBridge                                                                          | GCP Eventarc                                                                     |
| ---------- | ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------- |
| Strengths  | Best-in-class 99.99% SLA; native Azure resource event sourcing; CloudEvents standard [12] | Native transformation (Pipes); event archive + replay; 35+ SaaS partner sources [15] | Transformation pipelines; CloudEvents native; strong GCP service integration [4] |
| Weaknesses | No in-flight transformation (requires Lambda/Function workaround); no event replay [11]   | SLA not explicitly published; complex pricing with Pipes [15]                        | Smaller SaaS partner ecosystem; lower SLA (99.9%) than Event Grid [4]            |

### Narrative Analysis

While Azure Event Grid offers a superior 99.99% SLA, it historically lacked in-flight processing, requiring an external Azure Function for payload enrichment [11]. AWS EventBridge Pipes and GCP Eventarc Advanced now support native transformation pipelines, placing both AWS and GCP ahead in routing intelligence [15]. However, universal support for the CNCF CloudEvents standard across all three providers has significantly improved multi-cloud portability and reduced vendor lock-in [12]. For teams building purely within Azure, Event Grid's SLA and tight resource-lifecycle integration remain compelling advantages.

---

## Azure Event Hubs vs. AWS Kinesis vs. GCP Pub/Sub (Streaming)

### Overview

High-volume streaming platforms are distinct from messaging and event routing services, they are designed to ingest millions of events per second with low latency for telemetry, IoT, and real-time analytics pipelines [9].

- Azure Event Hubs: A partitioned, high-throughput ingestion service compatible with the Apache Kafka protocol. The backbone of real-time analytics on Azure [14].
- AWS Kinesis Data Streams: Amazon's managed data stream service offering shard-based parallelism, replay, and tight integration with the AWS analytics stack [9].
- GCP Pub/Sub: In its streaming mode, GCP Pub/Sub serves as both the event routing and high-throughput ingestion service, with direct integration into Dataflow and BigQuery [13].

### Core Features

| Feature             | Azure Event Hubs                                    | AWS Kinesis Data Streams                          | GCP Pub/Sub (Streaming)                        |
| ------------------- | --------------------------------------------------- | ------------------------------------------------- | ---------------------------------------------- |
| Throughput          | Millions of events/second via Throughput Units [14] | Millions of events/second via Shards [9]          | Millions of messages/second (auto-scaled) [13] |
| Partitioning Model  | Partitions (1–32 standard; up to 2000 Premium) [14] | Shards (manual scaling) [9]                       | Topics with automatic partitioning [13]        |
| Kafka Compatibility | Yes: Native Kafka-compatible interface [14]         | No: Requires Kafka on MSK for compatibility [9]   | No: Requires Kafka on Dataproc [13]            |
| Data Retention      | 1–7 days (standard); 90 days (Premium) [14]         | 24 hours (default); up to 365 days (extended) [9] | 7 days (configurable up to 31 days) [13]       |
| Replay Support      | Yes: Consumer groups with offset management [14]    | Yes: Sequence number-based replay [9]             | Yes: Snapshot + seek [13]                      |
| Message Size        | 1MB per event [14]                                  | 1MB per record [9]                                | 10MB per message [13]                          |

### Integration Options

- Event Hubs integrates with Azure Stream Analytics, Azure Databricks, Azure Data Factory, and downstream storage (ADLS Gen2, Blob Storage). Capture feature auto-archives to storage in Avro or Parquet format [14].
- Kinesis integrates with AWS Lambda, Kinesis Data Firehose (to S3/Redshift/Elasticsearch), Kinesis Data Analytics, and AWS Glue [9].
- GCP Pub/Sub integrates directly with Dataflow (Apache Beam), BigQuery (direct subscription), Cloud Storage, and Vertex AI. The tightest end-to-end analytics pipeline integration of the three [13].

### Monitoring & Observability

- Event Hubs: Azure Monitor metrics (incoming/outgoing messages, throttled requests, consumer lag), Diagnostic Logs, and Schema Registry metrics for schema evolution tracking [14].
- Kinesis: CloudWatch metrics (GetRecords.IteratorAgeMilliseconds, PutRecords.FailedRecords), Enhanced Monitoring for shard-level metrics, and Kinesis Data Analytics for real-time query monitoring [9].
- GCP Pub/Sub: Cloud Monitoring (oldest message age, subscription backlog), Cloud Logging, and Dataflow monitoring integration for pipeline health [13].

### Pricing Model

|                 | Event Hubs                                                               | AWS Kinesis                               | GCP Pub/Sub                            |
| --------------- | ------------------------------------------------------------------------ | ----------------------------------------- | -------------------------------------- |
| Model           | Throughput Units (Standard) or Processing Units (Premium) + storage [14] | Per shard-hour + per PUT payload unit [9] | Per GB ingested and delivered [13]     |
| Scaling Model   | Manual TU scaling or Auto-inflate [14]                                   | Manual shard splitting/merging [9]        | Fully auto-scaled (no shards/TUs) [13] |
| Kafka Advantage | Reuse Kafka clients without cluster management overhead [14]             | Must run separate MSK cluster [9]         | Must run separate Kafka cluster [13]   |

### Strengths & Weaknesses

|            | Event Hubs                                                                                                            | Kinesis                                                                                 | GCP Pub/Sub                                                                                      |
| ---------- | --------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| Strengths  | Kafka-compatible protocol eliminates migration risk; strong Azure analytics integration; auto-capture to storage [14] | Deepest AWS analytics integration; sequence-number replay; 365-day retention option [9] | Fully serverless (no partition management); largest message size; best BigQuery integration [13] |
| Weaknesses | Partition count fixed at creation (Premium required for high counts); Kafka parity is not 100% [14]                   | Manual shard management adds operational complexity; no native Kafka compatibility [9]  | No Kafka compatibility; auto-scaling can lead to unpredictable costs at extreme volume [13]      |

### Narrative Analysis

Both Azure Event Hubs and Amazon Kinesis scale to millions of events per second, serving as the backbone of real-time analytics when paired with Azure Databricks or Stream Analytics, and AWS Kinesis Data Analytics respectively [9]. The key differentiator for Event Hubs is its Kafka-compatible interface, which allows organizations to leverage the Kafka ecosystem without the management burden of a self-managed cluster [14]. GCP Pub/Sub's fully serverless, auto-partitioned model is operationally the simplest of the three, but its lack of Kafka compatibility makes it a migration target rather than a migration path for existing Kafka workloads.

---

## Summary Matrix

| Azure Service     | AWS Equivalent               | GCP Equivalent          | Strategic Fit                                      |
| ----------------- | ---------------------------- | ----------------------- | -------------------------------------------------- |
| Azure Functions   | Lambda                       | Cloud Functions         | Best ROI on existing Microsoft licenses [8]        |
| Durable Functions | Step Functions (Standard)    | Workflows               | Code-centric stateful orchestration [8]            |
| Logic Apps        | Step Functions + EventBridge | Application Integration | Visual low-code business process automation [8]    |
| Service Bus       | SQS + SNS                    | Pub/Sub                 | Financial and transactional message integrity [11] |
| Event Grid        | EventBridge                  | Eventarc                | Reactive Azure resource event routing [12]         |
| Event Hubs        | Kinesis Data Streams         | Pub/Sub (streaming)     | Big data ingestion; Kafka migration path [14]      |

### Architectural Recommendation Summary

- Azure is the optimal choice for organizations seeking maximum return on investment from existing Microsoft 365, Windows Server, and SQL Server investments [8].
- AWS remains the premier choice for organizations requiring the most granular infrastructure control, the broadest service ecosystem, and the highest global scale [1].
- GCP is the clear leader for cloud-native organizations prioritizing Kubernetes-native networking, advanced data analytics, and cost-effective AI model training through custom machine types [10].

## References (IEEE)

[1] N. Ahmad and L. Millares, "AWS vs Azure vs Google Cloud: Key Features and Pricing," Channel Insider, originally published Oct. 2024, updated Dec. 2025. [Online]. Available: [https://www.channelinsider.com/infrastructure/aws-vs-azure-vs-google-cloud/](https://www.channelinsider.com/infrastructure/aws-vs-azure-vs-google-cloud/)

[2] A. Abdullahi, "War-Driven Outages Put MSP Data Center Strategies at Risk," Channel Insider, Mar. 2026. [Online]. Available: [https://www.channelinsider.com/infrastructure/war-data-center-outages-msp-risk-strategy/](https://www.channelinsider.com/infrastructure/war-data-center-outages-msp-risk-strategy/)

[3] Amazon Web Services, "Amazon EventBridge pricing," AWS, 2026. [Online]. Available: [https://aws.amazon.com/eventbridge/pricing/](https://aws.amazon.com/eventbridge/pricing/)

[4] P. Leggetter, "Azure Event Grid Alternatives: Comparing Hookdeck Event Gateway, Amazon EventBridge, Google Eventarc, and Confluent Kafka," Hookdeck, originally published Apr. 12, 2024, updated Mar. 10, 2026. [Online]. Available: [https://hookdeck.com/webhooks/platforms/azure-event-grid-alternatives](https://hookdeck.com/webhooks/platforms/azure-event-grid-alternatives)

[5] nian2326076, "Azure Event Grid vs Service Bus vs Event Hubs," Reddit /r/softwarearchitecture, 2026. [Online]. Available: [https://www.reddit.com/r/softwarearchitecture](https://www.reddit.com/r/softwarearchitecture)

[6] Aress Software, "Cloud Pricing Comparison 2025: AWS vs. Azure vs. Google Cloud," Aress Software Blog, Mar. 25, 2025. [Online]. Available: [https://www.aress.com/blog/read/cloud-pricing-comparison-aws-vs-azure-vs-google-cloud](https://www.aress.com/blog/read/cloud-pricing-comparison-aws-vs-azure-vs-google-cloud)

[7] L. Gil, "Cloud Pricing Comparison: AWS vs. Azure vs. Google Cloud Platform in 2025," Cast AI Blog, Mar. 17, 2026. [Online]. Available: [https://cast.ai/blog/cloud-pricing-comparison-aws-vs-azure-vs-google-cloud-platform/](https://cast.ai/blog/cloud-pricing-comparison-aws-vs-azure-vs-google-cloud-platform/)

[8] Incredibuild Team, "Cloud services comparison: A practical developer guide," Incredibuild, Mar. 19, 2025. [Online]. Available: [https://www.incredibuild.com/blog/cloud-services-comparison](https://www.incredibuild.com/blog/cloud-services-comparison)

[9] G2, "Compare Amazon Kinesis Data Streams vs. Azure Event Hubs," G2, 2026. [Online]. Available: [https://www.g2.com/compare/amazon-kinesis-data-streams-vs-azure-event-hubs](https://www.g2.com/compare/amazon-kinesis-data-streams-vs-azure-event-hubs)

[10] M. Osman, "Comparing AWS, Azure, and GCP for Startups in 2026," DigitalOcean, Feb. 27, 2026. [Online]. Available: [https://www.digitalocean.com/resources/articles/comparing-aws-azure-gcp](https://www.digitalocean.com/resources/articles/comparing-aws-azure-gcp)

[11] Learnomate Technologies, "Event Grid vs Event Hub vs Service Bus in Azure – Complete Guide for Data Engineers," Learnomate Technologies, Jan. 2026. [Online]. Available: [https://learnomate.org/event-grid-vs-event-hub-vs-service-bus-azure/](https://learnomate.org/event-grid-vs-event-hub-vs-service-bus-azure/)

[12] ProjectPro, "Azure Event Grid vs AWS EventBridge: Which Tool is Better for Your Next Project?," ProjectPro, 2026. [Online]. Available: [https://www.projectpro.io/compare/azure-event-grid-vs-aws-eventbridge](https://www.projectpro.io/compare/azure-event-grid-vs-aws-eventbridge)

[13] ProjectPro, "Azure Event Hubs vs Google Cloud Pub/Sub: Which Tool is Better for Your Next Project?," ProjectPro, 2026. [Online]. Available: [https://www.projectpro.io/compare/azure-event-hubs-vs-google-cloud-pub-sub](https://www.projectpro.io/compare/azure-event-hubs-vs-google-cloud-pub-sub)

[14] ProjectPro, "Azure Event Hubs Projects," ProjectPro, 2026. [Online]. Available: [https://www.projectpro.io/projects/data-science-projects/azure-event-hubs-projects](https://www.projectpro.io/projects/data-science-projects/azure-event-hubs-projects)

[15] ProjectPro, "Google Cloud Pub/Sub vs AWS EventBridge: Which Tool is Better for Your Next Project?," ProjectPro, 2026. [Online]. Available: [https://www.projectpro.io/compare/google-cloud-pub-sub-vs-aws-eventbridge](https://www.projectpro.io/compare/google-cloud-pub-sub-vs-aws-eventbridge)