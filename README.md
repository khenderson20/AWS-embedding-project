# 📚 Amazon Titan Text Embeddings V2 Example

This Python script demonstrates how to generate and print an embedding with Amazon Titan Text Embeddings V2 using the Bedrock Runtime client.

## 📋 Prerequisites

Before running the script, please ensure you have the following prerequisites:

1. **Python**: The script is written in Python. You need to have Python 3.6 or later installed on your machine.

2. **AWS CLI-v2**: The script uses the AWS CLI to configure your AWS credentials. You can install the AWS CLI-v2 by following the instructions [here](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).

3. **Boto3**: The script uses the Boto3 library to interact with AWS services. You can install Boto3 using pip or however you are managing your Python packages I used `venv` to create a virtual enviornment initially but it wasn't needed in this example. Run the following command to install Boto3 using pip:

    ```bash
    pip install boto3
    ```

4. **AWS Account**: You need to have an AWS account and configure your AWS credentials on your machine. You can configure your AWS credentials by running `aws configure` in your terminal and following the prompts.

5. **AWS Permissions**: Your AWS account needs to have permissions to access the Bedrock Runtime service and the specific model (`amazon.titan-embed-text-v2:0`) used in this script.

6. **Connects to AWS services over the Internet**: The script needs to connect to AWS services over the internet.

Once you have all these prerequisites, you should be able to run the script without any issues.

## 📝 Script Overview

step 1. **AWS Client Setup**: The script begins by setting up a Bedrock Runtime client in the AWS Region of your choice.

```python
client = boto3.client("bedrock-runtime", region_name="us-east-1")
```

step 2. **Model ID**: The script sets the model ID to Amazon Titan Text Embeddings V2.

```python
model_id = "amazon.titan-embed-text-v2:0"
```

step 3. **Input Text**: The script sets the input text to be converted into an embedding.

```python
input_text = "Please recommend books with a theme similar to the movie 'Inception'."
```

step 4. **Request Creation**: The script creates a request for the model and converts it to JSON.

```python
native_request = {"inputText": input_text}
request = json.dumps(native_request)
```

step 5. **Model Invocation**: The script invokes the model with the request.

```python
response = client.invoke_model(modelId=model_id, body=request)
```

step 6. **Response Decoding**: The script decodes the model's native response body.

```python
model_response = json.loads(response["body"].read())
```

step 7. **Embedding Extraction**: The script extracts the generated embedding and the input text token count.

```python
embedding = model_response["embedding"]
input_token_count = model_response["inputTextTokenCount"]
```

step 8. **Printing**: Finally, the script prints the input text, the number of input tokens, the size of the generated embedding, and the embedding itself.

```python
print(f"\nYour input: {input_text}\n")
print(f"Number of input tokens: {input_token_count}")
print(f"Size of the generated embedding: {len(embedding)}")
print("Embedding:")
print(embedding)
```

## 🚀 Running the Script

To run the script, simply execute the `main.py` file in your Python environment.

```bash
python main.py
```

## 🎯 Expected Output

```bash
Your input: Please recommend books with a theme similar to the movie 'Inception'.

Number of input tokens: 15
Size of the generated embedding: 1024
Embedding:
[-0.024926797, 0.042596426, 0.009584196, 0.023822445, -0.00039194626, -0.04417407, 0.011359047, -0.03376161, -0.024453504, -0.033603847, -0.049538065, 0.014908749, -0.0320262, -0.01530316, 0.039441135, -0.017906275, -0.038652312, 0.03297279, 0.048907008, ...
```

*currently the input text is hardcoded in the script, but you can modify it to take input from a `.txt` file or `.pdf` file.*

## Ways to Expand on this Example

### Reading and Parsing Input text

**Reading from a File**:
You can expand on this example by reading the input text from a file instead of hardcoding it in the script. This would allow you to process large amounts of text data and generate embeddings for each text entry.

**Reading from a PDF**:
You can also read text from PDF files using libraries like `PyMuPDF` or `PyPDF2`. This would be useful if you have a collection of PDF documents that you want to process and generate embeddings for.

### Vector Databases

**Chroma DB**:
I was planning on using Chroma DB to store the embeddings and then query them to find similar embeddings. This would have been a great way to demonstrate how to use Chroma DB with Amazon Titan Text Embeddings V2. However, I ran out of time to implement this feature.

### Creating an API endpoint

**FastAPI**:
Another way to expand on this example would be to create an API endpoint using FastAPI that takes input text as a query parameter and returns the generated embedding. This would allow users to interact with the model through a simple API.

**simple FastAPI example**:

```python

# Import the FastAPI class from the fastapi module
from fastapi import FastAPI

# Import the BaseModel class from the pydantic module
from pydantic import BaseModel

# Create an instance of the FastAPI class
app = FastAPI()

# Define a Pydantic model named Text with a single attribute 'text' of type str
class Text(BaseModel):
    text: str

# Define an asynchronous route handler for the POST /embed route
@app.post("/embed")
async def embed_text(text: Text):
    # Call the get_embedding function with the text attribute of the input model
    embedding = get_embedding(text.text)
    # Return a dictionary with a single key 'embedding' and the list representation of the embedding as the value
    return {"embedding": embedding.tolist()}
```

This code defines a FastAPI application with a single route `/embed`. This route accepts POST requests with a JSON body that matches the `Text` model (i.e., a dictionary with a single key 'text' and a string as the value). The route handler function `embed_text` calls the `get_embedding` function with the text from the request body, converts the resulting embedding to a list, and returns it in a dictionary.

#### I plan to implement these features in the future and expand on this example further. I hope you find this example helpful and informative. If you have any questions or suggestions, please feel free to reach out to me. 🚀
