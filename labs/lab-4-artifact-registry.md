# Lab 4 - Artifact Registry

## Summary
This lab focuses on managing and securing your container images through Harness Artifact Registry. You'll learn how to configure your registries to automatically scan images for vulnerabilities, pull images from the registry, and leverage upstream proxies to control images pulled from public repositories. 

## Objectives
- Configure artifact registries for vulnerability scanning
- Set up upstream proxies for secure image pulling

## Why It Matters
This lab demonstrates how Harness Artifact Registry helps you secure your container images by automatically scanning them for vulnerabilities and providing secure image pulling through upstream proxies, reducing the risk of supply chain attacks.

## Steps
### Automatically scan images for vulnerabilities

**1.** From the Unified View, navigate to **Artifact Repositories --> Registries** in the left navigation bar

**2.** Click on the **har-<your_project_id>** registry.

**3.** Click the **Configuration** tab at the top to pull up the registry settings.

**4.** Find the **Security** configuration section. Turn on automated image scanning with one click by checking the box next to AquaTrivy.

**5.** Find the **Advanced (Optional)** configuration section and expand it. Notice the Org-level Upstream Proxy that's already been configured for you.

**6.** Click the **Save** button at the top of the page to save your changes.

![Registry Security Scanning](images/lab4-configure-registry-for-scanning.gif "Automated vulnerability scanning")

### Improved developer workflows - easily push and pull images

**1.** From the **har-<your_project_id>** registry, click the Artifacts tab to find the **harness-workshop** image and click on the link to the image. Expand the "-1" version and click on the digest hyperlink. Finally, click the **Set Up Client** button in the upper right corner.

**2.** On **Step 2** of the Docker Client Setup pop-up, click the **Generate Token** link to generate a token you'll use to authenticate to this registry. Paste the key in a text editor that can be referenced later.

![Setup Client](images/lab4-setup-client.gif "Setup Client Wizard")

**3.** Copy the provided Docker configuration commands and run them from your local machine or cloud shell to configure your Docker client to pull the **harness-workshop** image to your machine, re-tag it (e.g., `latest`), and push back to Harness Artifact Registry. E.g.:

```bash
docker login pkg.harness.io -u <your_workshop_username> -p <your_token>
docker pull pkg.harness.io/ifg41dwvsnarlovnb2uesg/har-<your_project_id>/harness-workshop:<your_project_id>-1
docker tag pkg.harness.io/ifg41dwvsnarlovnb2uesg/har-<your_project_id>/harness-workshop:<your_project_id>-1 pkg.harness.io/ifg41dwvsnarlovnb2uesg/har-<your_project_id>/harness-workshop:latest
docker push pkg.harness.io/ifg41dwvsnarlovnb2uesg/har-<your_project_id>/harness-workshop:latest
```

**4.** From the Unified View in the left navigation bar, click on **Pipelines** and you should see the Artifact Scan Pipeline running. This will take a couple minutes to complete.

**5.** While the pipeline is running, navigate back to your local machine terminal and pull an image that's publicly available on Docker Hub. Use the same URL from the previous Set Up Client wizard, substituting your public image for _harness-workshop:<tag>_. E.g.:

```bash
docker pull pkg.harness.io/ifg41dwvsnarlovnb2uesg/har-<your_project_id>/alpine:latest
```
From a developer experience perspective, developers have a single URL to use for any artifact they want to store or retrieve - public or private - reducing cognitive load and simplifying artifact management.

**6.** After the scan pipeline has finished, navigate back to _Artifact Registry --> Artifacts --> (Expand) **harness-workshop:latest** --> click on the digest hyperlink._ You should see the scan results under the "Vulnerabilities" tab. _Extra Credit: while you're here, take a look at the SBOM tab to understand the composition of the artifact we built, including the open source dependencies._

---

**Previous:** [Lab 3 - Continuous Deployment - Backend](lab-3-continuous-deploy-backend.md) · **Next:** [Lab 5: Validate Release](lab-5-validate-release.md)
