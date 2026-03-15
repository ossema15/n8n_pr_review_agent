n8n PR Review Agent
An n8n workflow that automates pull request reviews using AI. It analyses code diffs, flags bugs, suggests fixes, and auto-approves or requests changes — all without manual intervention.

What it does
When a pull request is opened, the workflow:

Fetches the diff from GitHub
Parses changed files and calculates PR size
Sends the diff to Google Gemini for code review
Posts a comment with a summary, issues found, and suggested fixes
Auto-approves the PR if the code is clean
Requests changes if issues are found
Sends a Slack notification with the result


Tech stack

n8n — workflow automation
GitHub API — trigger, comments, reviews
Google Gemini — AI code analysis
Slack — notifications
Docker — hosting n8n locally
ngrok — exposing n8n to the internet for webhooks


Setup
Prerequisites

Docker installed
ngrok account — ngrok.com
GitHub account + Personal Access Token (repo scope)
Google Gemini API key
Slack bot token (chat:write scope)


1. Run n8n with Docker
bashdocker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -e WEBHOOK_URL=<your_ngrok_url> \
  n8nio/n8n

2. Expose n8n with ngrok
ngrok is required for the GitHub webhook to reach your local n8n instance.
bash# Install ngrok from https://ngrok.com
# Then run:
ngrok http 5678
Copy the forwarding URL (e.g. https://abc123.ngrok.io) and set it as the WEBHOOK_URL environment variable when starting your Docker container.

3. Configure credentials in n8n
CredentialWhere to get itGitHub APIGitHub → Settings → Developer settings → Personal access tokensGoogle GeminiGoogle AI StudioSlackapi.slack.com/apps → Bot Token

4. Import the workflow

Open n8n at http://localhost:5678
Go to Workflows → Import
Upload the workflow JSON file from this repo


5. Set up GitHub webhook

Go to your repo → Settings → Webhooks → Add webhook
Payload URL: <your_ngrok_url>/webhook/<your-webhook-path>
Content type: application/json
Events: select Pull requests


Workflow nodes
NodeToolPurposeGitHub Triggern8n GitHub nodeFires on PR open / updateHTTP Requestn8n HTTP nodeFetches PR diff from GitHub APIDIFF Parsern8n Code nodeParses files, counts lines, extracts patchAI AgentGoogle GeminiReviews code, flags issues, suggests fixesCreate commentGitHub nodePosts AI review as PR commentIF noden8n IF nodeRoutes based on LGTM or NEEDS CHANGESCreate reviewGitHub nodeApproves or requests changesSlackSlack nodeSends notification to you

AI review format
The AI responds in this structure:
SUMMARY:
- What the PR does

ISSUES:
- Any bugs, security issues, or bad practices

SUGGESTED FIX:
- Corrected code snippet for each issue

VERDICT: LGTM or NEEDS CHANGES

Notes

n8n is hosted on Docker and exposed via ngrok — ngrok must be running for webhooks to work
You cannot request yourself as a reviewer on your own PR — use a second GitHub account for testing
Simple Memory is not needed for this workflow — each PR run is stateless
Make sure XS / S / M / L / XL labels exist in your GitHub repo before running
