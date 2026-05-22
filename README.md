\# Automated AWS Website Deployment



This project contains a static website hosted securely on AWS, provisioned using Infrastructure as Code (IaC) via AWS CloudFormation, and continuously deployed via GitHub Actions(CI/CD Pipeline).



\## 📁 Repository Structure



\*   `.github/workflows/deploy.yml` — GitHub Actions pipeline configuration.

\*   `cloudformation/template.yaml` — AWS Infrastructure as Code template.

\*   `images/` — Project documentation diagrams and architecture assets.

\*   `src/index.html` — Website source code files.



\## 🏗️ Architecture Overview



\### Diagram

!\[Cloud Architecture](images/Cloud%20Architecture%20.drawio.png)



\### Component Description

The system architecture automates the hosting and delivery of web content using AWS best practices:

\*   \*\*GitHub Actions\*\*: Triggers the automated pipeline on code updates, updating infrastructure and syncing code.

\*   \*\*AWS CloudFormation\*\*: Provisions and manages all underlying AWS resources deterministically.

\*   \*\*Amazon S3\*\*: Private storage origin bucket (`WebBucket`) holding static website files. Public access is entirely blocked.

\*   \*\*Amazon CloudFront\*\*: Acts as the Content Delivery Network (CDN) utilizing \*\*Origin Access Control (OAC)\*\* to securely fetch files from the private S3 bucket and serve them globally via HTTPS.



\## 🚀 Getting Started



\### Prerequisites



Before deploying, ensure you have the following:

1\. An active \*\*AWS Account\*\*.

2\. \*\*IAM Credentials\*\* configured with permissions for S3, CloudFront, and CloudFormation.

3\. A \*\*GitHub Repository\*\* to host this codebase.



\### GitHub Secrets Configuration



To enable the automated CI/CD pipeline, add the following secrets to your GitHub repository (\*\*Settings > Secrets and variables > Actions\*\*):



\*   `AWS\_ACCESS\_KEY\_ID`: Your AWS access key.

\*   `AWS\_SECRET\_ACCESS\_KEY`: Your AWS secret access key.

&#x20; 



\## 🔄 CI/CD Pipeline



The GitHub Actions workflow (`deploy.yml`) automates the entire deployment lifecycle. 



\### Trigger Workflow

The pipeline triggers automatically on any \*\*push\*\* to the `main` branch.



\### Pipeline Stages

1\. \*\*Checkout Code\*\*: Pulls the repository code into the runner environment.

2\. \*\*Configure AWS Credentials\*\*: Authenticates with AWS using repository secrets.

3\. \*\*Deploy CloudFormation Stack\*\*: Creates or updates the CloudFormation stack named `my-automated-website-stack`.

4\. \*\*Sync Website Assets \& Invalidate Cache\*\*: 

&#x20;   \* Captures outputs (`BucketName` and `DistributionId`) from the CloudFormation step.

&#x20;   \* Syncs the `./src` folder to the target S3 bucket while removing obsolete assets.

&#x20;   \* Clears the CloudFront CDN cache (`/\*`) so new website updates go live instantly.



&#x20; ## 🛠️ Development Workflow



To update your website content and trigger the live deployment pipeline, run the following commands in your local terminal(Git Bash):



1\. Track your changes:

&#x20;  ```bash

&#x20;  git add .

&#x20;  ```



2\. Commit your updates with a descriptive message using standard conventions:

&#x20;  ```bash

&#x20;  git commit -m "feat: update website content"

&#x20;  ```



3\. Push directly to the tracking branch to execute the automation:

&#x20;  ```bash

&#x20;  git push origin main



\## 🌐 How to Find Your Website URL



Once the GitHub Actions deployment completes successfully, follow these steps to find your live website link:



1\. Log in to the \[AWS Management Console](https://aws.amazon.com/free/?trk=011f28e3-1dfa-405d-894f-3e05b14038a6\&sc\_channel=ps\&trk=011f28e3-1dfa-405d-894f-3e05b14038a6\&sc\_channel=ps\&ef\_id=Cj0KCQjw\_b\_QBhCSARIsAP6hR4dvslK7IKx7FtgKlhO3Wgya696Gj1VsqqsdBXPApjW3KDWo4vLWLnkaAtVvEALw\_wcB:G:s\&s\_kwcid=AL!4422!3!808712687436!e!!g!!aws%20console!23846236274!198027693282\&gad\_campaignid=23846236274\&gbraid=0AAAAADjHtp-uX9AtzGeYeSqNa447dVsTX\&gclid=Cj0KCQjw\_b\_QBhCSARIsAP6hR4dvslK7IKx7FtgKlhO3Wgya696Gj1VsqqsdBXPApjW3KDWo4vLWLnkaAtVvEALw\_wcB).

2\. Search for and navigate to the \*\*CloudFormation\*\* service page.

3\. Click on the stack named \*\*`my-automated-website-stack`\*\*.

4\. Select the \*\*Outputs\*\* tab in the stack details panel.

5\. Locate the key named \*\*`WebsiteURL`\*\*.

6\. Click the URL link provided in the value field (e.g., `https://cloudfront.net`) to view your live site.

