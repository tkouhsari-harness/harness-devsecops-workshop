# Lab 4: Continuous Verification

## Summary
Canary deployments are great, but how do you know the canary is actually healthy? Continuous verification integrates with your observability tools and uses ML to compare metrics and logs against the baseline in real-time. No manual dashboard watching required. We'll also add chaos experiments to stress-test the deployment. If the canary survives intentional chaos, it's ready for production.

## Objectives
- Configure continuous verification to compare canary metrics against baseline using AI/ML
- Automate go/no-go decisions based on real-time observability data

## Why It Matters
This lab validates how Harness detects deployment issues based on real system behavior, not just pipeline success. Participants experience how deployments are continuously verified using telemetry + AI/ML, enabling faster detection and rollback of bad releases before they impact users, eliminating the need for manual monitoring, and reducing time-to-market.

## Steps

### Step 1: Replace Manual Approval with Continuous Verification

1. Select the **Deploy Backend** stage and delete the **Harness Approval** step
2. Hover the mouse between **Canary Deployment** and **Canary Delete** and then click the **+** icon to add a new step
3. Click **Add Step** and select the **AI Verify** step from the step library

   | Input                 | Value &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     | Notes &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                                                                                      |
   | ---------------------------- | -------- | ----- |
   | Name                         | <pre>AI Verify</pre> |       |
   | Continuous Verification Type | Canary   |       |
   | Duration                     | 10 min   |       |

4. Click **Save** and then click **Run** to execute the pipeline

   | Input       | Value  | Notes |
   | ----------- | ------ | ----- |
   | Branch Name | `main` |       |

---

### Step 2: Observe Canary Traffic

1. While the canary deployment is ongoing, navigate to the application URL
2. Under **Distribution Test**, click the **Start** button
3. Click the play button to run traffic generation
4. Click the refresh button to see the distribution of traffic hitting the canary pod
   <img src="https://lh7-us.googleusercontent.com/docsz/AD_4nXdbAmEJ5zQPsKlw_nEknWvYo97pm5eWCXr6vU8-GgIL0ulAOSH9N07PoEcVSknARVQo7Tgj1s31VHqR1I3hu2dMIO1rIX5HHcmTPXoQPoyo8CPv13OhnJN5WVcZqSwUXzdDHmm3PxUnhtpGVl0PAMJ_1wnuodvUbVPBOdnGKQ?key=cRG2cvp_PHVW0KG2Gq6Y_A" width="50%">
   <img src="https://lh7-us.googleusercontent.com/docsz/AD_4nXf-5oWX9OfvdmEb9MBm2_h2KKAa_QwmiJoM0fiKrTuxAr6GR4wxeulSlk48gyBK3dykrtIslDSkxpiGytrxH0JaxaQ4ZgTYxbmc8OenAH3nhGCvvOAxkWVjVBp1TRg_qQQi9z8OrNPK4udPtNL1LIyym6Ch5IMzrulFOcXhOQ?key=cRG2cvp_PHVW0KG2Gq6Y_A" width="50%">

---

### Step 3: Pipeline Execution

1. Return to the pipeline execution in Harness
2. Observe the outcome of the AI Verify step

Harness automatically evaluates metrics during the canary phase to determine if the application is healthy


**Previous:** [Lab 3: Continuous Deployment - Backend](lab-3-continuous-deploy-backend.md) · **Next:** [Lab 5: DevSecOps](lab-5-devsecops.md)
