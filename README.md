# Brain Tumor Prediction(CLoud Deplyment)
A serverless, cloud-native system that predicts the presence of a brain tumor from an uploaded MRI image in real time. Built as a Cloud Computing project to demonstrate secure, scalable deployment of a machine learning model using AWS-managed services end-to-end — from inference to logging to alerting.

🔗 Live App: https://main.d2n1xu0ezjn5n4.amplifyapp.com/

Overview
Brain tumors are among the most critical health challenges, and timely diagnosis significantly improves patient outcomes. Traditional MRI interpretation by specialists can be slow, error-prone, and inaccessible in remote or under-resourced areas.

This project addresses that gap by deploying a trained ML classifier as a fully serverless prediction pipeline on AWS — allowing a user to upload an MRI scan through a web frontend and receive an instant "Tumor Detected" / "No Tumor" prediction, with every request securely logged, monitored, and rate-alerted.

<img width="1009" height="701" alt="image" src="https://github.com/user-attachments/assets/658dfcd8-3cb0-4e5f-abba-6923b690c78e" />

Flow:
1.User uploads an MRI image via the Amplify-hosted frontend.
2.The request is routed through API Gateway to a Lambda function.
3.Lambda downloads the trained classifier model from S3, preprocesses the image, and runs inference.
4.The prediction result is stored in DynamoDB and returned to the frontend.
5.CloudWatch logs and monitors all activity for transparency and debugging.
6.SNS sends an automated alert if the API is invoked beyond a defined threshold.

AWS Services Used :
1.Amazon S3 – Securely stores the trained machine learning model artifact.
2.AWS Lambda – Serverless compute service that performs image preprocessing and executes prediction logic using Python 3.9.
3.Amazon API Gateway – Exposes a REST API endpoint (/predict) to invoke the Lambda function.
4.Amazon DynamoDB – NoSQL database used to store prediction results and metadata.
5.AWS IAM – Manages least-privilege access policies and permissions across AWS services.
6.Amazon CloudWatch – Provides real-time logging, monitoring, metrics, and alarms for the application.
7.Amazon SNS – Sends notifications when API usage exceeds a predefined threshold.
8.AWS Amplify – Hosts and automatically deploys the frontend application, integrated with the GitHub repository.

How prediction works?

1.The frontend captures the uploaded image and converts it to a Base64-encoded string.
2.This is sent as a JSON payload to the API Gateway /predict endpoint.
3.The Lambda function:
   -Decodes the Base64 image
   -Converts it to grayscale and resizes it to 64×64
   -Flattens the pixel data into a feature vector
   -Loads the trained classifier (.pkl, via joblib) from S3
   -Runs model.predict() and returns a human-readable label
4.The result — "Tumor Detected" or "No Tumor" — is stored in DynamoDB and displayed on the frontend.

<img width="892" height="359" alt="image" src="https://github.com/user-attachments/assets/5d7b0216-eb3a-44fc-902d-e7608639c88c" />

Future Improvements:
a.Migrate model hosting/inference to SageMaker for managed training and versioned deployment
b.Preserve image spatial structure (proper tensor shaping) to support true CNN-based inference rather than a flattened feature vector

