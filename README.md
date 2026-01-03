# AWS Rightsizing Report Generator

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![CloudFormation](https://img.shields.io/badge/CloudFormation-FF9900?style=flat&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/cloudformation/)

Automatically generate and distribute AWS Rightsizing Recommendation reports on a schedule. This tool bridges the gap between AWS Cost Explorer and your engineering teams by delivering actionable CSV reports directly to Email or Slack.

- [AWS Rightsizing Report Generator](#aws-rightsizing-report-generator)
  - [📖 Introduction](#-introduction)
  - [🚀 Key Features](#-key-features)
  - [🛠 Architecture](#-architecture)
  - [📋 Prerequisites](#-prerequisites)
  - [📦 Deployment](#-deployment)
  - [⚖️ License](#️-license)

## 📖 Introduction
Managing costs across multiple accounts is difficult when recommendations only show Account IDs. This project automates the generation of rightsizing reports, enriches them with **Account Names**, and sends them to your mailbox and to your Slack channel via SNS using secure **Pre-signed S3 URLs**.

## 🚀 Key Features
* **Dual-Report Output:** 
  * **Detailed Report:** Full metadata for deep-dive analysis.
  * **Summarized Report:** High-level overview for quick decision-making.
* **Account Name Enrichment:** Replaces cryptic Account IDs with friendly names for better readability.
* **Secure Distribution:** Delivers notifications via SNS (Email/HTTPS) with Pre-signed URLs (valid for 12 hours) to avoid public bucket exposure.
* **Automated Schedule:** Runs natively on the first Monday of every month via Amazon EventBridge.

## 🛠 Architecture
The CloudFormation stack deploys a serverless architecture:
* **AWS Lambda:** The engine that queries Cost Explorer API and generates CSVs.
* **Amazon S3:** Secure storage for generated reports.
* **Amazon EventBridge:** Cron-based trigger (1st Monday of the month).
* **Amazon SNS:** Fan-out notification system for Email and Slack/Webhook endpoints.

## 📋 Prerequisites
* **Account:** This template must be deployed in the **AWS Billing/Management Account** (or a delegated administrator account) to access cross-account cost data.
* **Service Access:** Ensure **Cost Explorer** is enabled in your AWS console.

## 📦 Deployment
1.  Log into the AWS Management Console of your **Billing Account**.
2.  Navigate to **CloudFormation** > **Create Stack**.
3.  Upload the `rightsizing-report.yaml` template.
4.  Fill in the required Parameters:
    * **S3 Bucket Name:** Unique name for report storage.
    * **Notification Email:** Email address to receive the monthly report links.
    * **Notification Webhook:** HTTPS webhook (Slack/Jira) to receive the monthly report links.
    * All other parameters are optional and can keep the default value.
5.  Click **Submit**.

## ⚖️ License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

