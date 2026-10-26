# Lab 1: Build

## Key Outcomes

- Configure a basic CI pipeline
- Run unit tests to validate code
- Build and push artifacts to a remote repository

## Overview

In this lab, the user sets up a CI pipeline that runs source code tests, builds the executable, and pushes the artifact to a remote Docker repository. This establishes the foundation for a deployable artifact that can be promoted through environments.

---

## Walkthrough

### Step 1: Create a Pipeline

1. In the Harness UI, from the left-hand side menu, navigate to **Projects** and select your assigned project
2. Use the [Changing Harness Module](../README.md#changing-harness-module) pre-read instruction to enable the **unified view**

   <img src="https://lh7-us.googleusercontent.com/docsz/AD_4nXfhuMykMsIHl-7FjliWssHc0uwRpdLdrnq7GkGAI0g6UBZM69F1zpQ8ZA8N_vMqjpoGFYFR_weJk7OtOGGa2bksIaS6BlktwytmuJ1THM3e8O6tDT18HYWwFyGUye8ubsrHBChI8ORrCQ88JcKWpLjQ0DsXDS0NSZrkfZ4RUQ?key=cRG2cvp_PHVW0KG2Gq6Y_A" width="50%">

3. From the left-hand side menu, select **Pipelines**
4. Click **+ Create a Pipeline**, enter the following values, then click **Start**

   | Input                 | Value &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     | Notes &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                                                                                      |
   | --------------------- | ---------- | ------------------------------------------------------------------------------------------ |
   | Name                  | <pre>workshop</pre>                                                                                   |
   | How do you want to setup your pipeline | Inline     | This indicates that Harness (rather than Git) will be the source of truth for the pipeline |

---

### Step 2: Add a Build Stage

1. From Pipeline Studio, click **Add Stage** and select **Build** as the Stage Type
2. Enter the following values and click **Set Up Stage**

    | Input                 | Value &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     | Notes &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                                                                                      |
    | --------------- | ------------------- | ----- |
    | Stage Name      | <pre>Build</pre>             |       |
    | Clone Codebase  | Enabled             |       |
    | Repository Name | `harness-devsecops` |       |

---

### Step 3: Configure Infrastructure

1. Navigate to the **Infrastructure** tab
2. Configure as follows:

    | Input                 | Value &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     | Notes &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                                                                                      |
    | -------------- | ----- | ----- |
    | Infrastructure | Cloud |       |

---

### Step 4: Configure Execution Steps

Navigate to the **Execution** tab and add the following steps:

**Test Intelligence Step**

1. Click **Add Step**, then **Add Step** again
2. Select **Test Intelligence** from the Step Library

    | Input   | Value                                    | Notes                                                                                                                                             |
    | ------- | ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name    | <pre>Run Tests With Intelligence</pre>            |                                                                                                                                                   |
    | Command | <pre>pip install pytest & cd ./python-tests</pre> | The GitHub repo is a monorepo with application(s) and configuration in the same repo. Therefore we need to navigate to the application subfolder. |

3. Click **Apply Changes**

**Compile Application (Template)**

1. Click **Add Step**, then **Use Template**
2. Select **Compile Application** and click **Use Template**

   | Input                 | Value &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     | Notes &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                                                                                      |
   | ----- | --------- | ----- |
   | Name  | <pre>Compile</pre> |       |

3. Click **Apply Changes**

**Build and Push to Docker**

1. Click **Add Step**, then **Add Step** again
2. Select **Build and Push an image to Docker Registry** from the Step Library

   | Input                      | Value                                             | Notes                                                                    |
   | -------------------------- | ------------------------------------------------- | ------------------------------------------------------------------------ |
   | Name                       | <pre>Push to DockerHub</pre>                               |                                                                          |
   | Registry Type              | Third Party Artifact Registry                     |                                                                          |
   | Docker Connector           | dockerhub                                         |                                                                          |
   | Docker Repository          | <pre>nikpap/harness-workshop</pre>                         |                                                                          |
   | Tags                       | <pre><+variable.username>-<+pipeline.sequenceId></pre>     | Click the +Add button under tags and then fill in with the value         |
   | **Optional Configuration** |                                                   |                                                                          |
   | Dockerfile                 | <pre>/harness/frontend-app/harness-webapp/Dockerfile</pre> | This tells Harness where the Dockerfile is for building the app          |
   | Context                    | <pre>/harness/frontend-app/harness-webapp</pre>            | This tells from where to run the instructions included in the Dockerfile |

3. Click **Apply Changes**

---

### Step 5: Save and Test the Pipeline

1. Click **Save** to finalize the pipeline
2. Click **Run** to manually execute it

   | Input       | Value  | Notes        |
   | ----------- | ------ | ------------ |
   | Branch Name | `main` | Prepopulated |

> [!WARNING]
> Pipeline execution is rejected by the policy engine. Create a compliant pipeline in [Lab 2: DevSecOps](lab-2-devsecops.md).

---

**Previous:** [Pre-Read](../README.md#lab-pre-read) · **Next:** [Lab 2: DevSecOps](lab-2-devsecops.md)
