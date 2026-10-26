# Lab 6: Governance / Policy as Code

## Key Outcomes

- Create policies that evaluate when editing pipelines
- Create policies that evaluate during pipeline execution
- Test policy enforcement

## Overview

In this lab, the user creates and applies policies as code to enable governance and promote self-service. Policies ensure that critical steps like approvals are always included in production pipelines.

---

## Walkthrough

### Step 1: Create a Policy to Require Approvals

1. From the left-hand side menu, hover over **Project Settings** and select **Policies** (towards the bottom of the 3rd column)
2. Click on the **Policies** tab
3. Click **+ New Policy**, enter the following values, then click **Apply**

   | Input       | Value                  | Notes |
   | ----------- | ---------------------- | ----- |
   | Policy Name | <pre>Require Approval for Production</pre> |       |

4. From the Sample Policies list on the right, select **Pipeline - Approval** and click **Use This Sample**
5. Click **Save**

---

### Step 2: Create a Policy Set

1. Click the **Policy Sets** tab
2. Click **+ New Policy Set**, enter the following values, then click **Continue**

   | Input            | Value                 | Notes |
   | ---------------- | --------------------- | ----- |
   | Policy Set Name  | <pre>Production Policies</pre> |       |
   | Entity Type that this policy set applies to      | Pipeline              |       |
   | On what event should the policy set be evaluated    | On Run                |       |

3. Click **+ Add Policy**
4. Select **Require Approval for Production**
5. Select **Error & Exit** for failure strategy
6. Click **Continue** and then click **Finish**
7. Toggle the **ENFORCED** button for the Policy Set you created

---

### Step 3: Test the Policy

1. Go to your pipeline and click **Run** to execute it
2. Note the failure due to lack of an approval stage

---

### Step 4: Add Approval Steps to Satisfy Policy

**Frontend Stage**

1. In the pipeline editor, go to the **Deploy Frontend** stage
2. Hover the mouse before the **Rollout Deployment** step and click the **+** icon to add a new step
3. Click **Add Step** and select the **Harness Approval** step from the step library

   | Input                 | Value &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     | Notes &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                                                                                      |
   | --------------- | ---------- | ----- |
   | Name        | <pre>Approval</pre>       |                                                      |
   | User Groups | All Project Users | Select project to see the "All Project Users" option |

4. Click **Apply Changes**

**Backend Stage**

1. In the pipeline editor, go to the **Deploy Backend** stage
2. Hover the mouse before the **Canary Deployment** step group and click the **+** icon to add a new step
3. Click **Add Step** and select the **Harness Approval** step from the step library

   | Input                 | Value &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     | Notes &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                                                                                      |
   | --------------- | ---------- | ----- |
   | Name        | <pre>Approval</pre>       |                                                      |
   | User Groups | All Project Users | Select project to see the "All Project Users" option |

4. Click **Apply Changes**

---

### Step 5: Test the Policy

1. Click **Run** to execute your pipeline
2. Note that the pipeline satisfies the policy and executes

---

## Value Callouts

| Aspect | Description |
| :--- | :--- |
| **Guardrails, Not Roadblocks** | Policies surface issues early without slowing down developers who follow best practices. |
| **Policy-as-Code** | Governance is defined in code and version-controlled—just like everything else. |
| **Scalable Compliance** | Teams can move fast while meeting security and audit requirements at scale. |

---

**Previous:** [Lab 5: Validate Release](lab-5-validate-release.md) · **Next:** [Lab 7: Governance / Policy as Code (Advanced)](lab-7-governance-policy-as-code-advanced.md)
