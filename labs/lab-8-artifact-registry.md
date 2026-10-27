# Lab 8: Artifact Registry

## Summary
This lab focuses on managing and securing your container images through Harness Artifact Registry. You'll learn how to configure your registries to automatically scan images for vulnerabilities, pull images from the registry, and leverage upstream proxies to control images pulled from public repositories. 

## Objectives
- Configure artifact registries for vulnerability scanning
- Set up upstream proxies for secure image pulling

## Why It Matters
This lab demonstrates how Harness Artifact Registry helps you secure your container images by automatically scanning them for vulnerabilities and providing secure image pulling through upstream proxies, reducing the risk of supply chain attacks.

## Steps
### Automatically scan images for vulnerabilities

1. From the left-hand side menu, hover over **Artifact Registry** and select **Registries**

2. Click on the **containers-<username>** registry

3. Click the **Configuration** tab at the top to pull up the registry settings.

4. Under the **Security** configuration section, turn on automated image scanning with one click by checking the box next to AquaTrivy.

5. Under the **Advanced (Optional)** configuration section and expand it. Notice the Organization level Upstream Proxy that's already been configured for you.

6. Click the **Save** button at the top of the page to save your changes.

### Improved developer workflows - easily push and pull images

1. From the **containers-<username>** registry, click the **Packages** tab
2. Click on the **workshop-app** image, then click on the latest digest
3. Click the **Set Up Client** button in the upper right corner
4. On **Step 2** of the Docker Client Setup drawer, click the **Generate Token** link to generate a token you'll use to authenticate to this registry. Paste the key in a text editor that can be referenced later.
5. Copy the provided Docker configuration commands and run them from your local machine or cloud shell to configure your Docker client to pull the **workshop-app** image to your machine, re-tag it (e.g., `latest`), and push back to Harness Artifact Registry. The commands should look something like the following.

    ```bash
    docker login pkg.harness.io -u <your_workshop_username> -p <your_token>
    docker pull pkg.harness.io/zja3ngzjnzytngzlny00yz/containers-<username>/workshop-app:<tag
    docker tag pkg.harness.io/zja3ngzjnzytngzlny00yz/containers-<username>/workshop-app:<tag> pkg.harness.io/zja3ngzjnzytngzlny00yz/containers-<username>/harness-workshop:latest
    docker push pkg.harness.io/zja3ngzjnzytngzlny00yz/containers-<username>/workshop-app:latest
    ```
6. From the Unified View in the left navigation bar, click on **Pipelines** and you should see the Artifact Scan Pipeline running. This will take a couple minutes to complete.
7. While the pipeline is running, navigate back to your local machine terminal and pull an image that's publicly available on Docker Hub. Use the same URL from the previous Set Up Client wizard, substituting your public image for _workshop-app:<tag>_. E.g.:

    ```bash
    docker pull pkg.harness.io/zja3ngzjnzytngzlny00yz/containers-<username>/alpine:latest
    ```
    From a developer experience perspective, developers have a single URL to use for any artifact they want to store or retrieve - public or private - reducing cognitive load and simplifying artifact management.

8. After the scan pipeline has finished, navigate back to the artifact registry and click on the latest digest

    You should see the scan results under the "Vulnerabilities" tab. 
    
    _Extra Credit: while you're here, take a look at the SBOM tab to understand the composition of the artifact we built, including the open source dependencies._

---

**Previous:** [Lab 7: Governance / Policy as Code (Advanced)](lab-7-governance-policy-as-code-advanced.md)
