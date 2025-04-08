### Grok LLM
[Link](https://docs.x.ai/docs/guides/image-generations#generate-image)

```bash
import os

from openai import OpenAI

XAI_API_KEY = os.getenv("XAI_API_KEY")
client = OpenAI(base_url="https://api.x.ai/v1", api_key=XAI_API_KEY)

response = client.images.generate(
  model="grok-2-image",
  prompt="A cat in a tree",
  response_format="b64_json"
)

print(response.data[0].b64_json)
```

### Groq LLM

```bash
curl https://api.groq.com/openai/v1/chat/completions -s \
-H "Content-Type: application/json" \
-H "Authorization: Bearer API_KEY" \
-d '{
"model": "meta-llama/llama-4-scout-17b-16e-instruct",
"messages": [{
    "role": "user",
    "content": "Explain the importance of fast language models"
}]
}'
```
