

# Retrieval-Augmented Generation (RAG) System (DataDynamo)

This repository provides a step-by-step guide to creating your own Retrieval-Augmented Generation (RAG) system. The system allows users to upload a PDF document, convert its content into a format usable by a language model, and generate context-aware responses using the extracted data.

## Features
- Upload PDF files and extract text content
- Store extracted data for retrieval
- Pre-prompt configuration to guide the AI model’s responses
- Generate AI-driven responses based on the provided data
- Customize pre-prompts and questions for accurate and contextually relevant responses

## Table of Contents
- [Installation](#installation)
- [Usage](#usage)
  - [Step 1: Upload PDF Data](#step-1-upload-pdf-data)
  - [Step 2: Convert to Numerical Format](#step-2-convert-to-numerical-format)
  - [Step 3: Store the Data](#step-3-store-the-data)
  - [Step 4: Set Pre-prompt](#step-4-set-pre-prompt)
  - [Step 5: Generate Responses with AI](#step-5-generate-responses-with-ai)
- [Customization](#customization)
- [License](#license)

## Installation

To use this system, you will need to install the following Python libraries:

```bash
pip install pdfplumber transformers
```

These libraries allow for PDF extraction and text generation.

## Usage

### Step 1: Upload PDF Data
First, you need to upload a PDF file containing the data you want to use.

### Step 2: Convert to Numerical Format
Extract the text from the PDF file and convert it into a format that can be used for data retrieval. The following code uses `pdfplumber` to extract text from each page of a PDF file.

```python
import pdfplumber

def extract_text_from_pdf(pdf_path):
    text = ""
    with pdfplumber.open(pdf_path) as pdf:
        for page in pdf.pages:
            text += page.extract_text() + " "
    return text

# Example usage
pdf_path = 'your_file.pdf'  # Replace with your PDF file path
pdf_text = extract_text_from_pdf(pdf_path)
print(pdf_text)  # This will print the extracted text from the PDF
```

### Step 3: Store the Data
Once the text is extracted, store the data in a structured format such as a list or dictionary.

```python
data_store = {
    "text": pdf_text.split('. ')  # Split the text into sentences
}
```

### Step 4: Set Pre-prompt
Create a pre-prompt that will help guide the language model in generating a relevant response. This pre-prompt will act as context for the model.

```python
pre_prompt = "Based on the following information, please provide a relevant answer: "
```

### Step 5: Generate Responses with AI
Now that the data is prepared and stored, you can use a generative AI model, like GPT-2 or another language model, to generate responses. The `transformers` library can help you achieve this.

```python
from transformers import pipeline

# Initialize the language model
generator = pipeline("text-generation", model="gpt-2")  # Replace with the model of your choice

def generate_response(user_query):
    context = " ".join(data_store['text'])  # Join the text from the data store
    full_prompt = pre_prompt + context + " Query: " + user_query
    
    # Generate the response
    response = generator(full_prompt, max_length=150)
    return response[0]['generated_text']

# Example usage
user_query = "What are the key points discussed in the document?"
response = generate_response(user_query)
print("AI Response:", response)
```

## Customization

### Customizing the Pre-prompt
You can change the pre-prompt to specify the type of response you want from the model. For example:

- For a concise answer:  
  `pre_prompt = "Provide a short and concise summary of the following text: "`
  
- For detailed guidance:  
  `pre_prompt = "Explain in detail the following key points from the text: "`

### Ask Your Questions
Once your data is ready and the model is set up, you can interact with it by asking questions. For example:

```python
user_query = "Summarize the main ideas from the document."
response = generate_response(user_query)
print(response)
```
