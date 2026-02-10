# AWS Orders → Payments Queue Architecture

![Architecture](diagrams/architecture.png)

## Project Goal
Build a resilient event-driven payment processing system that can handle traffic spikes without overloading downstream services by using queue buffering, idempotent processing, and asynchronous workflows.

## Architecture Overview
1. The **Order Lambda** creates an order record in **DynamoDB Orders** and sends a payment request to **SQS Orders Queue**.
2. **SQS** acts as a buffer, absorbing traffic spikes and decoupling services.
3. The **Payment Worker Lambda**, triggered by SQS, checks the **DynamoDB Idempotency table** to prevent duplicate processing.
4. The worker updates the order status to **PAID** and publishes an event to **SNS**.
5. **SNS** distributes notifications to subscribers such as email or downstream services.

## What This Demonstrates
- Queue-based backpressure protection using SQS
- Event-driven microservices architecture
- Idempotent “exactly-once-ish” processing
- Serverless scalability with AWS Lambda
- Reliable asynchronous communication using SNS