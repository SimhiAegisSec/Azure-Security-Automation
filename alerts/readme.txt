# Module 2: Real-time Incident Notification

## Workflow Architecture
[Azure Storage Account] -> [Azure Policy (Modify)] -> [Azure Monitor Alert] -> [Action Group] -> [Logic App] -> [Email Notification]

## Overview
While the Azure Policy automatically remediates security misconfigurations, it is critical for Security Operations (SecOps) teams to be notified of these events. This module implements a real-time alerting pipeline that triggers an email notification whenever the "Public Access" policy is enforced.
The automated workflow is defined in logic-app-template.json. It uses a serverless Consumption plan to keep costs at zero while providing real-time security notifications.

## Components
1. **Azure Logic App (Consumption):** A serverless workflow that receives an HTTP trigger and sends an email via Outlook/Gmail.
2. **Azure Monitor Action Group:** The "phone book" that tells Azure Monitor to call our Logic App URL.
3. **Azure Monitor Alert Rule:** The "listener" that watches the Activity Log for successful policy remediations.

## Deployment Guide

### 1. The Logic App
- **Trigger:** `When a HTTP request is received`
- **Action:** `Send an email`
- **Code:** The workflow definition is stored in `alerts/logic-app-template.json`.

### 2. The Alert Logic
- **Signal Name:** `Create/Update Storage Account (Microsoft.Storage/storageAccounts/write)`
- **Logic:** This alert fires specifically when the Policy Managed Identity finishes the 'Modify' operation on a Storage Account.
Note: The use of 'All' for Status and Event Level ensures maximum visibility during the development/PoC phase. In a high-traffic production environment, these would be filtered to 'Succeeded' only to reduce cost and alert noise.

## Repository Artifacts
- `logic-app-template.json`: The ARM template for the Logic App. To export this from the portal, go to your Logic App > **Export Template**.

##  Verification
1. Created an insecure Storage Account.
2. Verified Policy remediated the resource to 'Private'.
3. Received an email alert titled: "SECURITY ALERT: Automated Remediation Triggered" within 5 minutes of the event.

## Project Proof of Execution

Below are the screenshots confirming the successful automation of security remediation and notification.

### 1. Activity Log Event
The alert 'Fired' from Monitor Alerts
![Activity Log](./alerts/screenshots-mod2/proj2-mod2-alert-fired.png)

### 2. Logic App Execution
Proof that the Logic App workflow was triggered and completed successfully.
![Logic App Run History](./alerts/screenshots-mod2/proj2-mod2-logicapp-overview-succeeded-after.png)

### 3. Email Notification
The final email received via the automated pipeline.
![Email Notification](./alerts/screenshots-mod2/proj2-mod2-email-alert.png)
