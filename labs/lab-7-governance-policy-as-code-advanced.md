# Lab 7: Governance / Policy as Code (Advanced)

## Key Outcomes

- Block critical CVEs from reaching production
- Enforce security policies during runtime

## Overview

In this lab, you create an advanced policy that blocks deployments when critical vulnerabilities are detected by security scanners like OWASP.

---

## Walkthrough

### Step 1: Create a Policy to Block Critical CVEs

1. From the left-hand side menu, hover over **Project Settings** and select **Policies** (towards the bottom of the 3rd column)
2. Click on the **Policies** tab
3. Click **+ New Policy**, enter the following values, then click **Apply**

   | Input                 | Value &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     | Notes &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                                                                                      |
   | --------------- | ---------- | ----- |
   | Policy Name | <pre>Critical OWASP CVEs</pre> |       |

4. Enter the following REGO policy and click **Save**

   ```opa
   package pipeline_environment
   deny[sprintf("Node OSS Can't contain any critical vulnerability '%d'", [input.NODE_OSS_CRITICAL_COUNT])] {  
      input.NODE_OSS_CRITICAL_COUNT != 0
   }
   ```

---

### Step 2: Create a Policy Set

1. Click the **Policy Sets** tab
2. Click **+ New Policy Set**, enter the following values, then click **Continue**

   | Input                 | Value &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     | Notes &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                                                                                      |
   | --------------- | ---------- | ----- |
   | Policy Set Name  | <pre>Security Policies</pre> |       |
   | Entity Type that this policy set applies to      | Custom              |       |
   | On what event should the policy set be evaluated    | On Step                |       |

3. Click **+ Add Policy**
4. Select **Critical OWASP CVEs**
5. Select **Error & Exit** for failure strategy
6. Click **Continue** and then click **Finish**
7. Toggle the **ENFORCED** button for the Policy Set you created

---

### Step 3: Add Policy to Pipeline

1. Go to a previous execution of your pipeline and copy the **CRITICAL** output variable from the OWASP step like so:

![](https://lh7-us.googleusercontent.com/docsz/AD_4nXfYQ7ba5Q_cQ9xy2AFVZ5Mt0iZPYbyQDmBonp0pBQA13Z_IUeYdK8gRSbddtf_V3bSRfbhKWDbRSUVJTx3BTCc_VmwLIWyWLkdh89nLh0sEBA6fqQxTy0NADZ0YPZwCirNycRVGUQACdItaBotovPs5Hg6CmRpQHk5ysgV6RUlhSbIbkNxmHAo?key=cRG2cvp_PHVW0KG2Gq6Y_A)

2. Select the **Deploy Frontend** stage
3. Hover the mouse before **Harness Approval**, then click the **+** icon to add a new step
4. Click **Add Step** and select the **Policy** step from the step library

   | Input                 | Value &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     | Notes &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                                                                                      |
   | --------------- | ---------- | ----- |
   | Name        | <pre>Policy - No Critical CVEs</pre>     |                                                                                                          |
   | Entity Type | Custom                          |                                                                                                          |
   | Policy Set  | Security Policies           | Make sure to select the Project tab                                                                      |
   | Payload     | <pre>{"NODE_OSS_CRITICAL_COUNT": `<variable>` }</pre> | Set the field type to Expression, then replace `<variable>` with the OWASP output variable CRITICAL path |

5. Click **Apply Changes**

---

### Step 4: Test the Policy

1. Save the pipeline and execute
2. Note that the pipeline fails at the policy evaluation step due to critical vulnerabilities being found by OWASP

---

## Value Callouts

| Aspect | Description |
| :--- | :--- |
| **Security by Default** | Critical vulnerabilities are automatically blocked from production. |
| **Runtime Enforcement** | Policies evaluate real scan results, not just configuration. |
| **Risk Mitigation** | High-severity issues are caught before deployment, reducing attack surface. |
| **Compliance Automation** | Security policies are enforced consistently across all pipelines without manual oversight. |

---

**Previous:** [Lab 6: Governance / Policy as Code](lab-6-governance-policy-as-code.md)
