# Lab 3: Continuous Deployment - Backend

## Summary
Frontend is done. Now for the backend, where things can actually break in expensive ways. Let's use a canary deployment strategy with a manual approval before a broad rollout in order to minimize the blast radius. Deploy to a small slice of traffic, verify the canary is healthy, then promote to everyone. Progressive delivery made easy. 

## Objectives
- Extend the pipeline with multiple deployment stages for different services
- Implement advanced deployment strategies to reduce blast radius of a release
- Add manual approval gates and keep the human in the loop for controlled production releases

## Why It Matters
This lab validates Harness’s ability to safely deploy changes to production using advanced deployment strategies. Participants experience how risk is reduced through progressive delivery and manual validation — without complex scripting.
## Steps

### Step 1: Add Backend Deployment Stage

1. In the existing pipeline, click **Add Stage** and select **Deploy** as the Stage Type
2. Enter the following values and click **Set Up Stage**

   | Input                 | Value &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     | Notes &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                                                                                      |
   | --------------- | ---------- | ----- |
   | Stage Name      | <pre>Deploy Backend</pre>  |       |
   | Deployment Type | Kubernetes |       |

---

### Step 2: Configure Service

1. Click **- Select -** on the **Select Service** input box
2. Select **backend**

   | Input | Value     | Notes |
   | ----- | --------- | ----- |
   | Name  | `backend` |       |

3. Click **Apply Selected** and then click **Continue**

---

### Step 3: Configure Environment

The target infrastructure has been pre-created and was used in the previous stage. To reuse the same environment:

1. Click **- Propagate Environment From**
2. Select **Stage [Deploy Frontend]**
3. Click **Continue**

---

### Step 4: Configure Canary Deployment

1. Select **Canary** and click **Use Strategy**
2. Hover the mouse between **Canary Deploy** and **Canary Delete** and then click the **+** icon to add a new step
3. Select the **Harness Approval** step

   | Input                 | Value &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     | Notes &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                                                                                      |
   | --------------- | ---------- | ----- |
   | Name        | <pre>Approval</pre>       |          |
   | Message     | <pre>Please check the application at <+variable.lab_endpoint></pre>| |
   | User Groups | All Project Users | Select project to see the "All Project Users" option |

3. Click **Apply Changes**

---

### Step 5: Save and Run

1. Click **Save** and then click **Run** to execute the pipeline

   | Input       | Value  | Notes |
   | ----------- | ------ | ----- |
   | Branch Name | `main` |       |

While the canary deployment is ongoing and waiting for approval, navigate to the web page and see if you can spot the canary

1. Click on the **Approval** step
2. In the step details, click on the link next to the **Approve or Reject** button
3. In the app, click on the **Check Release** button
4. In the pipeline, click the **Approve or Reject** button and then click **Approve**

---

**Previous:** [Lab 2: Continuous Deployment - Frontend](lab-2-continuous-deploy-frontend.md) · **Next:** [Lab 4: Continuous Verification](lab-4-continuous-verification.md)
