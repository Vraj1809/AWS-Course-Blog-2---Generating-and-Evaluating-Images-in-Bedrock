# 📌 README: Image Generation, Evaluation, Modification & Storage using Amazon Titan & Nova Models on Bedrock

## 🚀 Project Overview
This project demonstrates a complete workflow with two distinct pipelines:

1. **AWS-based Pipeline:**  
   - **Image Generation & Evaluation on AWS:**  
     Utilizes Amazon Titan Image Generator G1 v2 for image generation and (intended) evaluation using Amazon Nova Lite.  
     **Storage:** The generated images (and evaluation data) are stored **only on Amazon S3** via AWS Lambda and API Gateway.  
     **Note:** Although we intended to obtain an evaluation score, for some reason we couldn't retrieve it. The underlying concept, however, is well understood.

2. **Local Python Pipeline:**  
   - **Image Generation & Modification using Python & Boto3 in VS Code:**  
     The user provides a text prompt and the system uses the Amazon Titan Image Generator G1 v2 to generate the image (without evaluation).
     **Storage:** The generated images are saved **locally** (in designated folders) **and** uploaded to **Amazon S3**.
     - Additionally, the image is modified using the Amazon Nova Canvas Model, with the modified image stored both locally and on S3.

This solution leverages **Amazon Bedrock** models for AI-driven image processing and uses **Python & Boto3** for local implementation and automation.

---

## 📥 Provisioning the Solution

1. **Download the CloudFormation YAML File:**
   - Get the sample YAML file from the following link:  
     [Download Sample YAML File](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?templateURL=https://aws-blogs-artifacts-public.s3.us-east-1.amazonaws.com/artifacts/ML-17361/bedrock-image-generation-evaluation-template.yml)

2. **Modify the YAML File:**
   - Open the downloaded YAML file in your preferred editor.
   - **Remove the line:** `concurrentExecution = 100`
   - **Update the Model ID and Payload:**
     - The original YAML file uses Claude Sonnet’s model ID. Since we are using **Amazon Nova Lite** for evaluation, update the model ID accordingly (for example, change it to the appropriate Nova Lite model ID).
     - Also, update the payload in the YAML file to match the input format expected by Amazon Nova Lite.
   - **Important Note:** Whenever you change the model, you must also modify the payload to conform to the new model’s requirements. Failure to do so will result in an evaluation error asking you to check and correct the input format.
   
3. **Deploy the Stack:**
   - Use AWS CloudFormation to deploy the modified YAML file, which provisions the necessary AWS Lambda, API Gateway, and S3 resources.

---

## 📷 Features Implemented

### 1. ✅ AWS-based Image Generation & (Intended) Evaluation
- **Image Generation:**  
  - **Model:** Amazon Titan Image Generator G1 v2  
  - The image is generated based on a user-provided prompt via an AWS Lambda function (proxied through API Gateway).
- **Image Evaluation (Conceptual):**  
  - **Model:** Amazon Nova Lite  
  - The plan was to evaluate the generated image (score, explanation, and improvement suggestions) using Nova Lite.  
  - **Note:** For some reason, the evaluation score couldn't be obtained, but the evaluation concept is fully understood.
- **Storage:**  
  - The generated image (and any evaluation details) are stored **exclusively in an Amazon S3 bucket**.
  
### 2. 🐍 Local Python Pipeline (VS Code)
- **Image Generation:**  
  - **Implementation:** Python & Boto3 in VS Code  
  - The user inputs a text prompt, and the system uses Amazon Titan Image Generator G1 v2 to generate the image.
  - **Storage:** The generated image is automatically saved in the `generated-images` folder on your local machine.
- **S3 Storage:**  
  - The generated image from the local pipeline is also uploaded to an S3 bucket, and a pre-signed URL is generated for easy access.
  
### 3. 🎨 Image Modification
- **Modification Process:**  
  - **Model:** Amazon Nova Canvas Model  
  - The original generated image is modified based on additional user prompts.
- **Storage of Modified Image:**  
  - **Local Storage:** The modified image is saved in the `updated-images` folder.  
  - **S3 Storage:** It is also uploaded to S3, providing a pre-signed URL for remote access.

---

## 🛠️ Technologies Used
- **Amazon Bedrock** (Titan Image Generator G1 v2, Nova Canvas, Nova Lite)
- **AWS Lambda & API Gateway** (for the AWS-based pipeline)
- **Amazon S3** (for storing generated and modified images)
- **Python & Boto3** (for scripting, automation, and local storage)
- **VS Code** (development environment)

---

## 📌 How to Run

### AWS-based Pipeline
1. **Deploy the AWS Resources:**  
   - Use the modified CloudFormation YAML file to deploy the necessary AWS Lambda, API Gateway, and S3 resources.
2. **Submit a Prompt:**  
   - **After deployment, navigate to API Gateway.**  
   - Locate the created API named **`BedrockImageGenEval`**.
   - Go to the **POST** method under this API.
   - Test the API by sending a JSON payload:  
     ```json
     { "prompt": "your prompt" }
     ```
3. **Results:**  
   - The image is generated (and intended to be evaluated) on AWS.
   - The generated image and any evaluation details are stored **only in the S3 bucket**.
   - Retrieve the output via the provided pre-signed S3 URL.

### Local Python Pipeline (VS Code)
1. **Run the Python Script in VS Code:**  
   - The script prompts you to enter a text prompt.
2. **Image Generation:**  
   - The image is generated using Amazon Titan Image Generator G1 v2.
3. **Local Storage:**  
   - The generated image is automatically opened and saved in the `generated-images` folder.
4. **S3 Storage:**  
   - The same image is uploaded to S3 and a pre-signed URL is generated.
5. **Image Modification:**  
   - Provide an additional prompt to modify the generated image using the Amazon Nova Canvas Model.
   - The modified image is saved locally in the `updated-images` folder and is also uploaded to S3.

---

## 🔗 Output Example

Below are examples of how you can reference and display the local images directly from the folder structure. Replace the sample file names with the actual file names generated by your system.

- #### **Local Generated Image:** 

  ![Local Generated Image](generated-images/62265cc6-0fe0-4198-9a57-9e8c09af448d.png)
  ![Local Generated Image](generated-images/f4cc658a-d828-4474-9819-dd6f25af8cd5.png)

- #### **Modified Image:**  

  ![Modified Image](updated-images/b2c5664e-2d94-40f6-a3ff-fa93faf47286.png)
  ![Modified Image](updated-images/dcd528d1-28e4-4473-ad0c-953c14edb576.png)

---

## 🧹 Cleanup
To avoid extra AWS costs, please ensure to:
- **Empty and delete the S3 bucket** used for storing images.
- **Delete the CloudFormation stack** if you used one for resource provisioning.

---

## 🎯 Conclusion
This project successfully demonstrates the **power and versatility** of Amazon Bedrock models for AI-driven media workflows. It features:
- An **AWS-based pipeline** for image generation and intended evaluation (stored only in S3).
- A **local Python pipeline** that stores generated images both locally and on S3.
- **Image modification** capabilities, with outputs available in both storage locations.

**Important:** Always remember, whenever you change the model in your CloudFormation template, you must update the input payload to match the new model's requirements. Failure to do so will result in an evaluation error asking you to correct the input format.

Together, these workflows showcase a seamless end-to-end process for generating, modifying, and accessing images through AWS services and Python scripting.

🚀 Happy Coding! 🎨📷
