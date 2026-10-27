# Lab 7: Governance / Policy as Code (Advanced)

## Summary
Our pipeline runs security scanners as part of the build process. This is a must as AppSec has enforced this via policy. However, we can still end up with critical vulnerabilities in production as there are no gates to stop based on findings.

## Objectives
- Block critical CVEs from reaching production
- Enforce security policies during runtime

## Why It Matters
This lab demonstrates how policies can be evaluated dynamically against the results of steps within the pipeline, allowing central teams to enforce policies on real-time data gathered throughout an execution.

## Steps

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

**Previous:** [Lab 6: Governance / Policy as Code](lab-6-governance-policy-as-code.md) · **Next:** [Lab 8: Artifact Registry](lab-8-artifact-registry.md)
