# Grok Imagine Image Quality API with APIDot

Build with the Grok Imagine Image Quality API using APIDot: cURL, Node.js, request shapes, polling, webhooks, and production notes.

[Try on APIDot](https://apidot.ai/models/grok-imagine-image-quality) | [Get API Key](https://apidot.ai/dashboard/api-key) | [API Docs](https://apidot.ai/docs/grok-imagine-image-quality) | [Pricing](https://apidot.ai/pricing) | [Main Examples](https://github.com/APIDotAI/apidot-examples)

## What this example shows

- How to submit Grok Imagine Image Quality image generation and edit-style jobs through APIDot.
- How to choose text-to-image or reference-guided image request shapes.
- How to set aspect ratio, resolution, output format, and output count.
- How to store `data.task_id`, poll status, and use webhook callbacks.

## When to use it

Use this repo when you need still-image generation or reference-guided image editing with the `grok-imagine-image-quality` model ID. It is a good starting point for product visuals, editorial mockups, marketing assets, and backend queues that need async image delivery.

This repo is for still image generation and editing. Use Grok video pages for video model IDs.

## Requirements

- An APIDot account.
- An APIDot API key stored server-side.
- Node.js 18 or newer for the Node.js example.
- `curl` for the cURL example.
- Public reference image URLs when using edit-style requests.

## Environment variables

Use placeholders only. Do not commit real credentials.

```env
APIDOT_API_KEY=YOUR_API_KEY_HERE
APIDOT_BASE_URL=https://api.apidot.ai
# Optional: set only when you have a real public webhook receiver.
# APIDOT_CALLBACK_URL=https://example.com/api/apidot/webhook
```

## How to run

```bash
cp .env.example .env
# Edit .env and set APIDOT_API_KEY
cd node
npm start
```

Use [curl/generate.md](curl/generate.md) when you want a copy-paste cURL request instead of the Node.js polling example.

## Minimal API request

Submit to APIDot's unified async generation endpoint:

```json
{
  "model": "grok-imagine-image-quality",
  "callback_url": "https://example.com/api/apidot/webhook",
  "input": {
    "prompt": "A premium editorial product photo of a transparent smart speaker on polished black glass, crisp readable logo, realistic studio reflections.",
    "resolution": "2K",
    "aspect_ratio": "1:1",
    "output_format": "png",
    "n": 1
  }
}
```

## Request variants

### Text-to-image

```json
{
  "model": "grok-imagine-image-quality",
  "input": {
    "prompt": "A realistic studio product photo of a compact desk lamp, soft shadows, clean background, readable brand mark.",
    "resolution": "1K",
    "aspect_ratio": "1:1",
    "output_format": "png",
    "n": 1
  }
}
```

### Reference-guided edit

```json
{
  "model": "grok-imagine-image-quality",
  "input": {
    "prompt": "Keep the product shape from the references and create a premium hero image on a clean reflective surface.",
    "image_urls": [
      "https://example.com/product-reference-1.webp",
      "https://example.com/product-reference-2.webp"
    ],
    "resolution": "2K",
    "aspect_ratio": "16:9",
    "output_format": "webp",
    "n": 1
  }
}
```

### Multiple candidates

```json
{
  "model": "grok-imagine-image-quality",
  "input": {
    "prompt": "Three visual directions for a premium product banner, clean typography space, realistic materials.",
    "resolution": "1K",
    "aspect_ratio": "3:2",
    "output_format": "jpeg",
    "n": 3
  }
}
```

## Request parameters

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| model | string | yes | Use `grok-imagine-image-quality`. |
| callback_url | string | no | Optional webhook callback URL for terminal task updates. |
| input.prompt | string | yes | Main instruction describing image, edit intent, composition, lighting, text, and style. |
| input.n | integer | no | Number of output images. Must be at least `1`. |
| input.aspect_ratio | string | no | Supported values are `1:1`, `2:3`, `3:2`, `9:16`, and `16:9`. |
| input.resolution | string | no | Supported values are `1K` and `2K`. |
| input.output_format | string | no | Supported values are `png`, `jpeg`, `jpg`, and `webp`. |
| input.image_urls | string[] | no | Optional reference image URLs for edit-style requests. Supports up to three images. |

## Expected response

Submit response:

```json
{
  "code": 200,
  "data": {
    "task_id": "task-unified-example",
    "status": "not_started",
    "created_time": "2026-04-19T21:19:42"
  }
}
```

Shortened finished response:

```json
{
  "code": 200,
  "data": {
    "task_id": "task-unified-example",
    "status": "finished",
    "output": {
      "files": [
        {
          "file_url": "https://example.com/generated-image.png",
          "file_type": "image"
        }
      ]
    }
  }
}
```

## Production notes

- Keep APIDot API keys in server-side environment variables.
- Persist `task_id`, prompt, reference image URLs, output settings, user ID, and status together.
- Validate aspect ratio, resolution, output format, and reference image count before submission.
- Use polling for local tests and webhook callbacks for production queues.
- Avoid logging API keys, private prompts, private media URLs, or production callback URLs.
- Retry transient network failures with backoff, but do not retry unchanged invalid payloads.

## Common mistakes

- Sending more than three reference image URLs.
- Sending unsupported aspect ratios, resolutions, or output formats.
- Treating this still-image endpoint as a video model.
- Losing `data.task_id` before polling or webhook reconciliation.
- Logging private prompts or reference media URLs.
- Committing a real `.env` file or API key.

## Related links

- Website: https://apidot.ai
- Docs: https://apidot.ai/docs
- GitHub: https://github.com/APIDotAI
- Grok Imagine Image Quality model page: https://apidot.ai/models/grok-imagine-image-quality
- Grok Imagine Image Quality API docs: https://apidot.ai/docs/grok-imagine-image-quality
- APIDot quickstart: https://apidot.ai/docs/quickstart
- APIDot webhooks: https://apidot.ai/docs/webhooks
- Main APIDot examples repo: https://github.com/APIDotAI/apidot-examples

## Promotion

- Topic: Grok Imagine Image Quality text-to-image and reference-guided request shapes with APIDot.
- Audience: Backend developers adding async image generation or editing to production workflows.
- Tweet angle: Grok Imagine Image Quality is a still-image workflow: choose prompt-only or up to three reference images, then set aspect ratio, resolution, and format.
- Landing page: https://apidot.ai/models/grok-imagine-image-quality
- Source status: verified

## Quality review

- Quality score: 38/40
- Promotion candidate: yes
- Blocking issues: None for request-shape and async workflow promotion. Verify the model page and docs again before making cost, ranking, or availability claims.
