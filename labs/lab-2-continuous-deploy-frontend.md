# Lab 2: Continuous Deployment - Frontend

## Summary: 
Our application compiled successfully and the artifact is in the Harness Artifact Registry. Time to deploy it. Extend the pipeline to ship the frontend to a Kubernetes cluster using a rolling deployment. The manifests are ready, no manual kubectl commands, no deployment scripts to maintain, just point Harness at your manifests and let it handle the rest.

## Objectives

- Extend CI pipelines with Continuous Deployment stages
- Define Kubernetes services with manifests and artifact sources
- Use Harness expressions for dynamic artifact tagging
- Implement rolling deployment strategies

## Why It Matters
This lab demonstrates how teams can quickly and easily deploy software without custom scripting, leveraging native rolling deployment capbilities. The lab goes under the hood to show what teams deploy (the Harness Service) and where they deploy it (the Harness Environment) are decoupled from the deployment logic defined in the pipeline. This decoupled architecture unlocks pipeline standardization at scale. 

## Steps

### Step 1: Add a Deployment Stage

1. In the existing pipeline, click **Add Stage** and select **Deploy** as the Stage Type
2. Enter the following values and click **Set Up Stage**

   | Input                 | Value &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     | Notes &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                                                                                      |
   | --------------- | ---------- | ----- |
   | Stage Name      | <pre>Deploy Frontend</pre> |       |
   | Deployment Type | Kubernetes |       |

---

### Step 2: Configure Service

1. Click **+ Add Service** and configure as follows:

   | Input                      | Value                                         | Notes                                                                        |
   | -------------------------- | --------------------------------------------- | ---------------------------------------------------------------------------- |
   | Name                       | <pre>frontend</pre>                           |                                |
   | Deployment Type            | Kubernetes                                    |                                |
   | **Add Manifest**           |                                               |                                |
   | Manifest Type              | K8s Manifest                                  |                                |
   | K8s Manifest Store         | Code                                          |                                |
   | Manifest Identifier        | <pre>templates</pre>                          |                                |
   | Repository                 | harness-devsecops                  |                                |
   | Branch                     | <pre>main</pre>                               |                                |
   | File/Folder Path           | <pre>harness-deploy/frontend/manifests</pre>  |                                |
   | Values.yaml                | <pre>harness-deploy/frontend/values.yaml</pre>| Click **+Add File** then enter the value                                |
   | **Add Artifact Source**    |                                               |                                |
   | Artifact Repository Type   | Artifact Registry                               |                                |
   | Artifact Source Identifier | <pre>frontend</pre>                           |                                |
   | Registry | `containers-<username>` |  |
   | Package Name               | <pre>workshop-app</pre>            |                                |
   | Tag                        | <pre><+variable.username>-<+pipeline.sequenceId></pre> | Click on the sigma button, select expression, then paste the value |

2. Click **Save** to close the service window and then click **Continue**

---

### Step 3: Configure Environment

The target infrastructure has been pre-created. The application will be deployed to a Kubernetes cluster on the given namespace.

1. Click **- Select -** on the **Specify Environment** input box
2. Select **prod** environment and click **Apply Selected**

   | Input                 | Value &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     | Notes &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                                                                                      |
   | ----- | ------ | ----------------------------------------------------------------- |
   | Name  | `prod` | Make sure to select the environment and infrastructure definition |

3. Click **- Select -** on the **Specify Infrastructure** input box
4. From the dropdown select **k8s**

   | Input                 | Value &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     | Notes &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                                                                                      |
   | ----- | ----- | ----- |
   | Name  | `k8s` |       |

5. Click **Continue**

---

### Step 4: Configure Execution Strategy

1. Select **Rolling** and click **Use Strategy**
2. The frontend is a static application, so no need for canary deployment

---

### Step 5: Save

1. Click **Save** to save the pipeline

---

**Previous:** [Lab 1: Continuous Integration - Build](lab-1-build.md) · **Next:** [Lab 3: Continuous Deployment - Backend](lab-3-continuous-deploy-backend.md)
