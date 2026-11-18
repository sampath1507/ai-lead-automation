# AI Lead Automation — Portfolio Demo

An importable **n8n lead-capture workflow** for a small business. A POST webhook receives an inquiry, validates the data, classifies its priority and topic, and returns a structured JSON response. This first version uses **transparent rule-based classification, not a language model**; an AI model integration is a planned enhancement.

## Features
- Webhook-based lead intake
- Required name, email, and message validation
- Basic email-format validation
- Rule-based topic and priority tagging
- JSON success/error responses
- No paid APIs, cloud hosting, or credentials needed for the local demo

## Quick start
1. Install Docker Desktop and start it.
2. Run `docker compose up -d` in this repository.
3. Open http://localhost:5678 and complete n8n's local setup.
4. Import `workflows/lead-intake.json` via **Import from File**.
5. Click **Listen for test event** on the Webhook node.
6. In another terminal, run:
   ```bash
   curl -X POST http://localhost:5678/webhook-test/lead-intake \
     -H "Content-Type: application/json" \
     -d '{"name":"Alex Example","email":"alex@example.com","message":"We urgently need a website quote","source":"website"}'
   ```
7. Inspect the returned JSON. For production-path testing, activate/publish the workflow and use `/webhook/lead-intake` instead of `/webhook-test/lead-intake`.

**Note:** n8n versions may use slightly different workflow activation labels. Local n8n data persists in a Docker volume.

## Example
Input: `examples/sample-lead.json`

Expected fields in a successful response: `ok`, `lead` (including `name`, `email`, `message`, `source`, `topic`, `priority`, `receivedAt`). Invalid inputs return `ok: false` and an `error` message.

## Architecture
```
Website / curl POST
       |
   n8n Webhook
       |
Validate & Classify (JavaScript Code node)
       |
 Respond to Webhook (JSON)
```

## Portfolio honesty
This is an original **self-initiated demonstration**, not a paid client engagement. The initial version does not yet write to Google Sheets, send emails, or call an LLM. Those can be added after the core workflow is tested.

## Security notes
- Run locally; do not expose the webhook publicly without authentication, abuse prevention, and privacy safeguards.
- Use fictional demo leads only.
- Do not commit credentials, tokens, personal customer data, or local n8n state.
- Review the n8n license before embedding or redistributing n8n as part of a commercial product.

## Roadmap
- [ ] Add Google Sheets integration using the user's own OAuth credentials
- [ ] Add email notification node with an approved provider
- [ ] Optional AI classifier with a configurable provider
- [ ] Add authentication and rate limiting for a public endpoint
- [ ] Record a demo video and screenshots

## License
MIT for the original files in this repository. n8n itself has its own license.
