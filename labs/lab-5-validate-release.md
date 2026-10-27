# Lab 5: Validate Release

## Key Outcomes

- Identify traffic differences between normal and canary instances
- Automate release validation

## Overview

In this lab, you validate canary deployments by observing traffic distribution and monitoring application behavior during progressive rollouts.

---

## Walkthrough

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


---

### Bonus Activities

- If verification fails, Harness defaults to manual intervention. Decide what happens next (rollback, ignore, etc.)
- Add a canary rollout from 10% to 50% traffic and observe how this impacts traffic distribution

---

## Value Callouts

| Aspect | Description |
| :--- | :--- |
| **Automated Validation** | Harness validates deployments automatically, eliminating manual post-deployment monitoring. |
| **Traffic Shaping** | Canary deployments allow controlled traffic distribution for risk mitigation. |

---

**Previous:** [Lab 4 - Artifact Registry](lab-4-artifact-registry.md) · **Next:** [Lab 6: Governance / Policy as Code](lab-6-governance-policy-as-code.md)
