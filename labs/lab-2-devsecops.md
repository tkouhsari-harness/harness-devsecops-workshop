# Lab 2: DevSecOps

## Key Outcomes

- Understand how governance plays a role in the path to production
- Leverage reusable templates to standardize security scanning
- Achieve DevSecOps practices with minimal developer friction

## Overview

In this lab, the security team has implemented orchestration of **Fortify** and **OWASP** scans in reusable templates. Policies enforce the inclusion of these templates in all pipelines, ensuring security scans are always executed before deployment.

<img src="https://lh7-us.googleusercontent.com/docsz/AD_4nXcLr5TGcKRWOjVgB_sCAHHEeLPyd6EBdnkt2-mq_imTkZbQMEwJD03Q1wZyhWqHxoCNIIYWJWlRbnZrvZn2pPYIwTzXlOGdhMDEgn-J2JnK7lVastmfpdwTqDHXjpP0DK3TgU1gM-Ec_0iZLicWV7KpgW2FdXUCcAtraDGaEz8hI3dpWGLXkg?key=cRG2cvp_PHVW0KG2Gq6Y_A" width="50%">

---

## Walkthrough

### Step 1: Add Security Scanning Steps

1. Click the Edit Pipeline (pencil) button at the top right of the execution.
2. In the **Build** stage, hover the mouse between **Compile** and **Push to DockerHub** and then click the **+** icon to add a new step
3. Select **Use Template**

   ![](https://lh7-us.googleusercontent.com/docsz/AD_4nXeC5rTVxlk7DeZeU_cINwcKo6Nf2wVW9brQ9MiCEfppJwmU-uH3QcNZ53qTxhur57KeySksoDBg9EqjhgKOgAEDKon6iNz9cFxozBe9VZssV-t77VNo6t1zPUvm6e2NOZJDKncxd9c2GM4HE-h-L4cIOl4u6Uqx_azoKchMdg?key=cRG2cvp_PHVW0KG2Gq6Y_A)

4. Select **DevX Fortify Scan** and click **Use Template**

   | Input                 | Value &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     | Notes &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                                                                                      |
   | ----- | --------- | ----- |
   | Name  | <pre>Fortify</pre> |       |

5. Click **Apply Changes**

---

### Step 2: Add OWASP Scan

1. Hover the mouse between **Fortify** and **Push to DockerHub** and then click the **+** icon to add a new step
2. Select **Use Template**
3. Select **OWASP** and click **Use Template**

   | Input                 | Value &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     | Notes &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                                                                                      |
   | ----- | ------- | ----- |
   | Name  | <pre>OWASP</pre> |       |

4. Click **Apply Changes**

---

### Step 3: Run the Pipeline

1. Click **Save** and then **Run** to execute the pipeline

   | Input       | Value  | Notes |
   | ----------- | ------ | ----- |
   | Branch Name | `main` |       |

2. After the **Build and Push** stage is complete, navigate to the **Vulnerabilities** tab to see the deduplicated, normalized, and prioritized list of vulnerabilities discovered across your scanners

---

## Value Callouts

| Aspect | Description |
| :--- | :--- |
| **Shift Left Security** | Security scans run during the build phase, catching vulnerabilities before deployment. |
| **Template Reusability** | Security steps are defined once and reused across all pipelines, ensuring consistency. |
| **Policy Enforcement** | Governance policies ensure security scans cannot be skipped. |
| **Unified View** | All vulnerabilities are deduplicated and prioritized in a single dashboard. |

---

**Previous:** [Lab 1: Build](lab-1-build.md) · **Next:** [Lab 3: Continuous Deploy - Frontend](lab-3-continuous-deploy-frontend.md)
