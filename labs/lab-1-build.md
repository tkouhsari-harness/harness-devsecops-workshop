# Lab 1:  Continuous Integration - Build

## Summary

Setup a CI Pipeline, including running source code tests, building the executable, and building and pushing the artifact to Harness Artifact Registry.

## Objectives:

- Understand how to configure a basic pipeline using Harness CI
- Review how to build and deploy an artifact to an artifact repository using Harness CI
- Understand how unit tests are integrated into the build process using Harness CI

## Why It Matters:

This lab establishes the starting point of the software delivery lifecycle and ensures artifacts entering the deployment process are consistent, repeatable, and traceable. While CI is not the focus of this POC, this lab validates that Harness cleanly integrates with existing CI workflows and produces deployable artifacts that downstream CD and Artifact Registry processes can rely on.

## Steps

### Step 1: Create a Pipeline

1. In the Harness UI, from the left-hand side menu, navigate to **Projects** and select your assigned project
2. Use the [Changing Harness Module](../README.md#changing-harness-module) pre-read instruction to enable the **unified view**

   ![](https://lh7-us.googleusercontent.com/docsz/AD_4nXfhuMykMsIHl-7FjliWssHc0uwRpdLdrnq7GkGAI0g6UBZM69F1zpQ8ZA8N_vMqjpoGFYFR_weJk7OtOGGa2bksIaS6BlktwytmuJ1THM3e8O6tDT18HYWwFyGUye8ubsrHBChI8ORrCQ88JcKWpLjQ0DsXDS0NSZrkfZ4RUQ?key=cRG2cvp_PHVW0KG2Gq6Y_A)
3. From the left-hand side menu, select **Pipelines**
4. Click **+ Create a Pipeline**, enter the following values, then click **Start**
   | Input                                  | Value    | Notes                                                                                      |
   | -------------------------------------- | -------- | ------------------------------------------------------------------------------------------ |
   | Name                                   | workshop |                                                                                            |
   | How do you want to setup your pipeline | Inline   | This indicates that Harness (rather than Git) will be the source of truth for the pipeline |

---

### Step 2: Add a Build Stage

1. From Pipeline Studio, click **Add Stage** and select **Build** as the Stage Type
2. Enter the following values and click **Set Up Stage**
   | Input           | Value               | Notes |
   | --------------- | ------------------- | ----- |
   | Stage Name      | Build               |       |
   | Clone Codebase  | Enabled             |       |
   | Repository Name | `harness-devsecops` |       |

---

### Step 3: Configure Infrastructure

1. Navigate to the **Infrastructure** tab
2. Configure as follows:
   | Input                 | Value                                         | Notes                                                                                                                          |
     | -------------- | ----- | ----- |
     | Infrastructure | Cloud |       |

---

### Step 4: Configure Execution Steps

Navigate to the **Execution** tab and add the following steps:

**Test Intelligence Step**

1. Click **Add Step**, then **Add Step** again
2. Select **Test Intelligence** from the Step Library
   | Input   | Value                                  | Notes                                                                                                                                             |
   | ------- | -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
   | Name    | Run Tests With Intelligence            |                                                                                                                                                   |
   | Command | pip install pytest & cd ./python-tests | The GitHub repo is a monorepo with application(s) and configuration in the same repo. Therefore we need to navigate to the application subfolder. |
3. Click **Apply Changes**

**Compile Application (Template)**

1. Click **Add Step**, then **Use Template**
2. Select **Compile Application** and click **Use Template**
   | Input | Value   | Notes |
   | ----- | ------- | ----- |
   | Name  | Compile |       |
3. Click **Apply Changes**

**Build and Push to Docker**

1. Click **Add Step**, then **Add Step** again
2. Select **Build and Push an image to Docker Registry** from the Step Library
   | Input                      | Value                                           | Notes                                                                                                                  |
   | -------------------------- | ----------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
   | Name                       | Push to Harness AR                              |                                                                                                                        |
   | Registry Type              | Artifact Registry                               |                                                                                                                        |
   | Registry                   | containers-                                     |                                                                                                                        |
   | Package Name               | workshop-app                                    | Because this image doesn't already exist, you'll need to first type this, then click **+workshop-app** in the dropdown |
   | Tags                       | <+variable.username>-<+pipeline.sequenceId>     | Click the +Add button under tags and then fill in with the value                                                       |
   | **Optional Configuration** |                                                 |                                                                                                                        |
   | Dockerfile                 | /harness/frontend-app/harness-webapp/Dockerfile | This tells Harness where the Dockerfile is for building the app                                                        |
   | Context                    | /harness/frontend-app/harness-webapp            | This tells from where to run the instructions included in the Dockerfile                                               |
3. Click **Apply Changes**

---

### Step 5: Save and Test the Pipeline

1. Click **Save** to save the pipeline
2. Click **Run** to manually execute it
   | Input       | Value  | Notes        |
    | ----------- | ------ | ------------ |
    | Branch Name | `main` | Prepopulated |
3. Wait for the pipeline to finish
4. From the left-hand side menu, hover over **Artifact Registry** and click on **Registries**
5. Select the **containers-** registry and inspect the package that was just built and pushed to the registry

---

**Previous:** [Pre-Read](../README.md#lab-pre-read) · **Next:** [Lab 2: Continuous Deployment - Frontend](lab-2-continuous-deploy-frontend.md)
