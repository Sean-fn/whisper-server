# Whisper API Client Usage Guide

This README provides a guide on how to interact with the Whisper API using Python.

## Setup

Before you begin, make sure you have Python and the `requests` library installed. If you don't have the `requests` library installed, you can install it using pip:

```sh
pip install requests
```

## Configuration

Set your Whisper API credentials and the base URL for the API:

```python
USERNAME = 'your_username'
PASSWORD = 'your_password'
BASE_URL = 'https://whisper.yang-lin.dev'
```

## Authentication

The Whisper API uses Basic Authentication. Include your credentials in each request:

```python
from requests.auth import HTTPBasicAuth

auth = HTTPBasicAuth(USERNAME, PASSWORD)
```

## Uploading a File

To upload a file, send a POST request to `/api/upload` with the file included in the request body as form-data:

```python
import requests

def upload_file(file_path):
    url = f'{BASE_URL}/api/upload'
    with open(file_path, 'rb') as file:
        files = {'file': file}
        response = requests.post(url, files=files, auth=auth)
        return response.json()
```

Usage:

```python
file_path = 'path/to/your/file'
upload_response = upload_file(file_path)
print(upload_response)
```

## Processing a File

To process a file, send a GET request to `/api/process/<file_id>` with the file ID obtained from the upload step:

```python
def process_file(file_id):
    url = f'{BASE_URL}/api/process/{file_id}'
    response = requests.get(url, auth=auth)
    return response.json()
```

Usage:

```python
file_id = upload_response['file_id']
process_response = process_file(file_id)
print(process_response['whisper_text']) # The processed text

```

## Notes

- Ensure your file size is under 100MB to avoid both timeouts and restrictions of our CDN server.
- Processing time may vary based on file size and content.
- In case of issues, reach out to Whisper API support.

For further details, refer to the official Whisper API documentation.
