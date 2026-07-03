# Grok Imagine Image Quality cURL Quickstart

## What this example shows

This example submits one still-image task and then polls the shared APIDot task status endpoint.

## How to run

```bash
export APIDOT_API_KEY="YOUR_API_KEY_HERE"

curl --fail-with-body --request POST \
  --url https://api.apidot.ai/api/generate/submit \
  --header "Authorization: Bearer $APIDOT_API_KEY" \
  --header "Content-Type: application/json" \
  --data '{
    "model": "grok-imagine-image-quality",
    "input": {
      "prompt": "A premium editorial product photo of a transparent smart speaker on polished black glass, crisp readable logo, realistic studio reflections.",
      "resolution": "2K",
      "aspect_ratio": "1:1",
      "output_format": "png",
      "n": 1
    }
  }'
```

Store the returned `data.task_id`, then poll status:

```bash
curl --fail-with-body --request GET \
  --url https://api.apidot.ai/api/generate/status/task-unified-example \
  --header "Authorization: Bearer $APIDOT_API_KEY"
```

