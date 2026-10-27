# Lab 5: DevSecOps

## Summary
We've got our application into production and automatically verified that things look good from an observability perspective. But what if we've introduced vulnerabilities into prod? In fact, AppSec is going to start blocking builds and deployments if we don't run security scans.

## Objectives
- Understand how governance plays a role in the path to production
- Leverage reusable templates to standardize security scanning
- Achieve DevSecOps practices with minimal developer friction

## Why It Matters
This lab validates how easily security scanning can be added into the pipeline, how the results from all scanners are automatically gathered, deduplicated/normalized/prioritized and presented in an easy to understand view for developers, and how policy as code can be used to enforce required steps.

---

## Steps

### Step 1: Enable Policy

1. From the left-hand side menu, hover over **Project Settings** and select **Policies** (towards the bottom of the 3rd column)
2. Click on the **Policy Sets** tab
3. Toggle the **Enforced** button for `<username>-policyset`

   This Policy Set includes a Policy that enforces that security scanning is included in the pipeline

### Step 2: Run Pipeline

1. Go back to the pipeline and click the **Run** button

   > Pipeline execution is rejected. Policy as Code saves the day!

### Step 3: Add Security Scanning Steps

1. Click the Edit Pipeline (pencil) button at the top right of the execution.
2. In the **Build** stage, hover the mouse between **Compile** and **Push to Harness AR** and then click the **+** icon to add a new step
3. Select **Use Template**
4. Select **DevX Fortify Scan** and click **Use Template**

   | Input                 | Value &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     | Notes &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                                                                                      |
   | ----- | --------- | ----- |
   | Name  | <pre>Fortify</pre> |       |

5. Click **Apply Changes**

---

### Step 4: Add OWASP Scan

1. Hover the mouse between **Fortify** and **Push to DockerHub** and then click the **+** icon to add a new step
2. Select **Use Template**
3. Select **OWASP** and click **Use Template**

   | Input                 | Value &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     | Notes &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                                                                                      |
   | ----- | ------- | ----- |
   | Name  | <pre>OWASP</pre> |       |

4. Click **Apply Changes**

---

### Step 5: Run the Pipeline

1. Click **Save** and then **Run** to execute the pipeline

   | Input       | Value  | Notes |
   | ----------- | ------ | ----- |
   | Branch Name | `main` |       |

2. After the **Build** stage is complete, navigate to the **Vulnerabilities** tab to see the deduplicated, normalized, and prioritized list of vulnerabilities discovered across your scanners

---

**Previous:** [Lab 4: Continuous Verification](lab-4-continuous-verification.md) · **Next:** [Lab 6: Governance / Policy as Code](lab-6-governance-policy-as-code.md)
