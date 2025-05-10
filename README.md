
# 🚀 Azure-Based Serverless AI Text Generation API Using Hugging Face Transformers, Azure Machine Learning, Azure Functions, and Postman

## Overview

This project demonstrates how to build a serverless AI text generation API on Microsoft Azure. It involves deploying a Hugging Face Transformer model using Azure Machine Learning, creating an Azure Function to serve the model, and testing the API with Postman.

---

## 🧱 Architecture

- **Model Hosting**: Azure Machine Learning (AML) for deploying the Hugging Face Transformer model.
- **Serverless Function**: Azure Functions to expose the model as an HTTP endpoint.
- **API Testing**: Postman for sending requests and receiving responses from the API.

---

## 🧰 Prerequisites

- An active [Azure subscription](https://azure.microsoft.com/free/).
- [Azure CLI](https://learn.microsoft.com/cli/azure/install-azure-cli) installed.
- [Azure Functions Core Tools](https://learn.microsoft.com/azure/azure-functions/functions-run-local) installed.
- [Postman](https://www.postman.com/downloads/) installed.
- Basic knowledge of Python and familiarity with Azure services.

---

## 📦 Repository Structure

```bash
azure-textgen-api/
├── azureml/
│   ├── deploy_model.py
│   └── score.py
├── azure_function/
│   ├── __init__.py
│   └── function.json
├── requirements.txt
└── README.md
```

---

## 🛠️ Step-by-Step Guide

### 1. Set Up Azure Machine Learning Workspace

Create a new AML workspace or use an existing one. Ensure you have a compute instance or cluster available for deploying the model.

### 2. Deploy the Hugging Face Model

Use the `deploy_model.py` script to register and deploy the model:

```bash
python azureml/deploy_model.py
```

This script performs the following:
- Registers the Hugging Face model.
- Creates an inference configuration using `score.py`.
- Deploys the model to an Azure Container Instance (ACI) or Azure Kubernetes Service (AKS).

### 3. Develop Azure Function

Navigate to the `azure_function/` directory and initialize a new Azure Function:

```bash
func init --python
func new --name TextGenFunction --template "HTTP trigger"
```

Replace the contents of `__init__.py` with code that sends a request to the deployed AML model endpoint. Ensure you include the necessary authentication headers.

### 4. Configure `function.json`

Set up the HTTP trigger and route in `function.json`:

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": ["post"]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ]
}
```

### 5. Deploy Azure Function

Deploy the function to Azure:

```bash
func azure functionapp publish <YourFunctionAppName>
```

Ensure that the function app has the necessary environment variables set to authenticate and communicate with the AML model endpoint.

### 6. Test with Postman

- Open Postman and create a new POST request to the Azure Function URL.
- Set the request body to JSON with the input text:

```json
{
  "input": "Once upon a time"
}
```

- Send the request and observe the generated text in the response.

---

## 🧪 Sample Request & Response

**Request:**

```json
{
  "input": "The future of AI is"
}
```

**Response:**

```json
{
  "generated_text": "The future of AI is promising, with advancements in machine learning and deep learning leading to more intelligent systems."
}
```

---

## 📌 Notes

- Ensure that the Azure Function has the necessary permissions to access the AML endpoint.
- Monitor the function and model deployment for any errors or performance issues.
- Consider implementing authentication and rate limiting for the API in a production environment.

---

## 📚 References

- [Azure Machine Learning Documentation](https://learn.microsoft.com/azure/machine-learning/)
- [Azure Functions Documentation](https://learn.microsoft.com/azure/azure-functions/)
- [Hugging Face Transformers](https://huggingface.co/transformers/)
- [Postman Documentation](https://learning.postman.com/docs/getting-started/introduction/)

---

This README provides a comprehensive guide to deploying a serverless AI text generation API on Azure, adapting the original AWS-based project to Azure's ecosystem.
