# Module 2: Real-time Incident Notification

## Overview
While the Azure Policy automatically remediates security misconfigurations, it is critical for Security Operations (SecOps) teams to be notified of these events. This module implements a real-time alerting pipeline that triggers an email notification whenever the "Public Access" policy is enforced.

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
- **Signal Name:** `Create or Update Remediation (Microsoft.PolicyInsights/remediations)`
- **Logic:** This alert fires specifically when the Policy Managed Identity finishes the 'Modify' operation on a Storage Account.

## Repository Artifacts
- `logic-app-template.json`: The ARM template for the Logic App. To export this from the portal, go to your Logic App > **Export Template**.

##  Verification
1. Created an insecure Storage Account.
2. Verified Policy remediated the resource to 'Private'.
3. Received an email alert titled: "SECURITY ALERT: Automated Remediation Triggered" within 5 minutes of the event.
