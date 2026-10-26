# Lab 4: Continuous Deploy - Backend

## Key Outcomes

- Utilize complex deployment strategies to reduce blast radius
- Implement approval gates for production deployments

## Overview

In this lab, you add a backend deployment stage using a canary deployment strategy with manual approval gates to minimize risk during production releases.

---

## Walkthrough

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
2. **After** the canary deployment and **before** the canary delete step, add a **Harness Approval** step

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

---

## Value Callouts

| Aspect | Description |
| :--- | :--- |
| **Progressive Delivery** | Canary deployments reduce risk by gradually rolling out changes. |
| **Manual Gates** | Approval steps ensure human oversight for critical deployments. |
| **Environment Propagation** | Reusing environments across stages reduces configuration duplication. |

---

**Previous:** [Lab 3: Continuous Deploy - Frontend](lab-3-continuous-deploy-frontend.md) · **Next:** [Lab 5: Validate Release](lab-5-validate-release.md)
