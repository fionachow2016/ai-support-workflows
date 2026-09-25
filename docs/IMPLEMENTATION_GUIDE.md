# Implementation Guide: AI Support Operations Workflow

**Last Updated:** 2026-09-25  
**Audience:** Implementation Lead, IT/Ops Team, Support Manager  
**Time to Complete:** 2-3 hours initial setup + 15 min per team member training

---

## Prerequisites Checklist

Before starting, verify you have:

- [ ] Zendesk account with API access enabled
- [ ] Claude API key (from api.anthropic.com)
- [ ] Access to shipstation.com public knowledge base
- [ ] 50+ person support team ready to adopt
- [ ] Jira Service or Zendesk as primary ticketing system
- [ ] Internal wiki/Confluence (for internal KB articles)

---

## Phase 1: Infrastructure Setup (30 min)

### Step 1.1: Get Claude API Access

1. **Create account:**
   - Go to https://api.anthropic.com
   - Sign up or log in
   - Navigate to "API Keys"

2. **Generate API key:**
   - Click "Create API Key"
   - Save securely (you'll only see it once)

3. **Store in environment:**
   ```bash
   # On your server, set environment variable:
   export ANTHROPIC_API_KEY="sk-ant-xxxxxxxxxxxx"
   
   # Or in .env file (never commit to git):
   ANTHROPIC_API_KEY=sk-ant-xxxxxxxxxxxx
   ```

4. **Test connection:**
   ```python
   import anthropic
   client = anthropic.Anthropic()
   response = client.messages.create(
       model="claude-3-5-sonnet-20241022",
       max_tokens=100,
       messages=[{"role": "user", "content": "Say 'API working!'"}]
   )
   print(response.content[0].text)
   # Should output: "API working!"
   ```

### Step 1.2: Enable Zendesk API

1. **In Zendesk admin:**
   - Settings → Apps → API Token

2. **Create new token:**
   - Click "Add API Token"
   - Name: "ShipStation-AI-Workflows"
   - Copy the token
   - Note your subdomain (e.g., `shipstation-support`)

3. **Store credentials:**
   ```bash
   export ZENDESK_SUBDOMAIN="shipstation-support"
   export ZENDESK_EMAIL="ai-workflows@shipstation.com"
   export ZENDESK_API_TOKEN="eyJhbGciOiJIUzI1NiJ9..."
   ```

4. **Test connection:**
   ```python
   import requests
   url = f"https://{subdomain}.zendesk.com/api/v2/users/me.json"
   headers = {"Authorization": f"Bearer {api_token}"}
   response = requests.get(url, headers=headers)
   print(response.status_code)  # Should be 200
   ```

### Step 1.3: Set Up Vector Database (KB Search)

We recommend **Pinecone** (free tier supports 1M vectors):

1. **Create Pinecone account:**
   - Go to https://www.pinecone.io
   - Sign up (free tier)
   - Create new index: `support-kb`
   - Dimension: 1536 (for Claude embeddings)

2. **Get API key:**
   - Save your API key and environment

3. **Store credentials:**
   ```bash
   export PINECONE_API_KEY="your-api-key"
   export PINECONE_ENVIRONMENT="us-west1-gcp"
   export PINECONE_INDEX="support-kb"
   ```

---

## Phase 2: Knowledge Base Setup (45 min)

### Step 2.1: Scrape ShipStation Public Docs

Run the KB scraper:

```bash
# Clone or download the scraper script
git clone https://github.com/shipstation/ai-workflows.git
cd ai-workflows

# Install dependencies
pip install -r requirements.txt

# Scrape ShipStation docs and index in Pinecone
python scripts/scrape_shipstation_kb.py \
  --url https://shipstation.com/help \
  --output kb_articles.json

# Index in vector database
python scripts/index_kb.py \
  --source kb_articles.json \
  --destination pinecone \
  --batch-size 100
```

**Expected output:**
```
✓ Scraped 287 articles from shipstation.com
✓ Cleaned and formatted 287 articles
✓ Embedded 287 articles (took 45 sec)
✓ Indexed in Pinecone
✓ Verified 287 articles searchable
```

**Verify:**
```python
# Test search
from scripts.kb_search import search_kb
results = search_kb("tracking not updating")
print(f"Found {len(results)} articles")
# Should return top 3-5 matching articles
```

### Step 2.2: Set Up Internal KB (Optional But Recommended)

Add your company-specific knowledge:

1. **Create Confluence space** (or Google Drive folder):
   - Name: "ShipStation Support KB"
   - Permissions: Read-only for agents, edit for managers

2. **Create initial articles:**
   - Company refund policy
   - Escalation procedures
   - Support SLAs
   - Team-specific playbooks

3. **Export and index:**
   ```bash
   python scripts/index_internal_kb.py \
     --source confluence \
     --space "support-kb" \
     --destination pinecone
   ```

---

## Phase 3: Zendesk Automation Setup (45 min)

### Step 3.1: Create Custom Fields

In Zendesk, create these custom fields:

| Field Name | Type | Values |
|---|---|---|
| `ai_classification` | Text | (AI fills this) |
| `ai_urgency` | Dropdown | Critical, High, Medium, Low |
| `ai_category` | Dropdown | Shipping, Billing, Integration, Account, Technical, General |
| `ai_sentiment` | Dropdown | Frustrated, Neutral, Positive |
| `ai_confidence` | Number | 0-100 |
| `response_type` | Dropdown | ai_sent_as_is, ai_customized, human_written |

### Step 3.2: Create Triage Automation Rule

In Zendesk: Settings → Automations → Create New

**Trigger:** "Ticket is created"

**Actions:**
1. Call webhook to Claude API:
   ```
   URL: https://your-api.com/api/triage
   Method: POST
   
   Payload:
   {
     "ticket_id": "{{ticket.id}}",
     "subject": "{{ticket.title}}",
     "description": "{{ticket.description}}",
     "zendesk_token": "{{env.ZENDESK_API_TOKEN}}"
   }
   ```

2. AI responds with classification:
   ```json
   {
     "category": "shipping",
     "urgency": "high",
     "sentiment": "frustrated",
     "confidence": 92,
     "suggested_response": "..."
   }
   ```

3. Zendesk automation:
   ```
   Update ticket:
   - Tag: #{{classification.category}}
   - Tag: #{{classification.urgency}}
   - Field "ai_urgency": {{classification.urgency}}
   - Field "ai_category": {{classification.category}}
   - Move to queue: {{classification.queue}}
   - Add internal note with suggested response
   ```

### Step 3.3: Create Response Draft Display

Add widget to Zendesk ticket view:

```html
<!-- Save as: zendesk_widget.html -->
<div id="ai-response-draft">
  <h3>AI-Generated Response Draft</h3>
  <div id="kb-articles">
    <h4>Recommended KB Articles:</h4>
    <ol id="article-list"></ol>
  </div>
  <div id="response-body">
    <h4>Draft Response:</h4>
    <textarea id="response-text"></textarea>
  </div>
  <button id="send-btn">Send Response</button>
  <button id="edit-btn">Edit & Customize</button>
  <button id="reject-btn">Reject & Rewrite</button>
</div>

<script>
// JavaScript to fetch and display AI response
fetch(`/api/ticket/${ticketId}/suggested-response`)
  .then(r => r.json())
  .then(data => {
    document.getElementById('article-list').innerHTML = 
      data.kb_articles.map(a => 
        `<li><a href="${a.url}">${a.title}</a> (${a.confidence}%)</li>`
      ).join('');
    document.getElementById('response-text').value = data.response;
  });

document.getElementById('send-btn').onclick = () => {
  // Send response
};
</script>
```

---

## Phase 4: Deploy Triage Service

### Option A: Cloud Deployment (Recommended)

Deploy on Google Cloud Run (scales automatically):

```bash
# 1. Containerize
docker build -t ai-triage:latest .
docker tag ai-triage gcr.io/[PROJECT_ID]/ai-triage:latest
docker push gcr.io/[PROJECT_ID]/ai-triage:latest

# 2. Deploy to Cloud Run
gcloud run deploy ai-triage \
  --image gcr.io/[PROJECT_ID]/ai-triage:latest \
  --platform managed \
  --region us-central1 \
  --memory 512Mi \
  --timeout 30s \
  --set-env-vars ANTHROPIC_API_KEY=$ANTHROPIC_API_KEY,\
ZENDESK_TOKEN=$ZENDESK_API_TOKEN,\
PINECONE_API_KEY=$PINECONE_API_KEY

# 3. Note the endpoint:
# Service deployed to: https://ai-triage-xxxxxx.run.app
```

### Option B: Self-Hosted

Deploy on your own server:

```bash
# 1. Clone repo
git clone https://github.com/shipstation/ai-workflows.git
cd ai-workflows

# 2. Install dependencies
pip install -r requirements.txt

# 3. Start service
python -m uvicorn app:app --host 0.0.0.0 --port 8000

# 4. Use reverse proxy (nginx):
# Forward /api/triage → localhost:8000
```

---

## Phase 5: Team Training (15 min per agent)

### Training Agenda (15 minutes)

1. **Show the dashboard** (2 min)
   - Live demo: https://claude.ai/artifact/[LINK]
   - Show how triage works
   - Show how response drafts are generated

2. **Walk through Ticket Triage runbook** (4 min)
   - What AI does automatically
   - Why tickets are routed to certain queues
   - How to fix misclassification

3. **Walk through Response Drafting runbook** (6 min)
   - How to review AI drafts
   - When to customize vs. send as-is
   - Quality checklist
   - Common mistakes to avoid

4. **Walk through KB Search runbook** (2 min)
   - How KB articles are auto-included
   - How to search manually if needed
   - How to report KB gaps

5. **Q&A** (1 min)

### Training Materials

Send team:
- 📊 Dashboard link: https://claude.ai/artifact/734MDStRtPPQ5Kv8gYhT1B
- 📖 Three runbooks (attached)
- 🎥 Recording of this training (optional)
- 📋 Quick reference card (see below)

### Training Checklist

Before going live, verify:
- [ ] All agents have seen the dashboard
- [ ] All agents have read the 3 runbooks
- [ ] All agents understand: AI helps, agents review
- [ ] All agents know how to report issues
- [ ] Team lead knows how to monitor metrics

---

## Phase 6: Soft Launch (Day 1-3)

### Day 1: "Silent Mode"
- Deploy to production
- Run in "log-only" mode (no auto-routing yet)
- Review accuracy of AI classifications
- Fix any obvious bugs

**Check:**
```bash
# Review logs
tail -f /var/log/ai-triage.log | grep "classification"

# Sample output:
# [INFO] Ticket #12345 classified as: shipping/high/frustrated (confidence: 94%)
# [INFO] Ticket #12346 classified as: billing/medium/neutral (confidence: 87%)
```

### Day 2-3: "Training Mode"
- Enable auto-routing, but agents can easily override
- Monitor for misclassifications
- Collect feedback from first 100 tickets
- Make any quick adjustments

**Metrics to monitor:**
```
- Triage accuracy: 287 tickets processed, 263 correct = 91.6%
- Response time: avg 2.3 sec from ticket arrival to draft ready
- Agent satisfaction: "Is AI helpful?" feedback
- Adoption rate: % of agents using AI drafts
```

---

## Phase 7: Full Launch (Day 4+)

Once you hit 90%+ triage accuracy:

1. **Enable all automation**
   - Auto-routing fully active
   - Response drafts for all tickets

2. **Monitor dashboard:**
   - Track metrics daily first week
   - Weekly after that

3. **Gather feedback:**
   - 1:1s with agents: "What's working? What's not?"
   - Team meeting: "How can we improve?"

4. **Iterate:**
   - Retrain model monthly on misclassifications
   - Improve KB articles based on usage
   - Adjust rules based on feedback

---

## Post-Launch: Ongoing Operations

### Weekly (Support Manager)
- [ ] Review triage accuracy report (target: >92%)
- [ ] Review response quality metrics
- [ ] Check for new KB gaps
- [ ] Monitor agent feedback

### Monthly (All Roles)
- [ ] Team retro: "What AI improvements help most?"
- [ ] Update KB with new articles
- [ ] Retrain model on misclassifications
- [ ] Review cost (Claude API usage)

### Quarterly (Leadership)
- [ ] Present metrics to leadership
- [ ] Plan next improvements
- [ ] Analyze ROI: time saved, quality improvements
- [ ] Identify expansion opportunities (other teams?)

---

## Troubleshooting Checklist

| Issue | Debug Steps | Solution |
|---|---|---|
| Tickets not getting routed | Check Zendesk automation active? Check webhook logs? | Restart automation, check API keys |
| AI classifications wrong | Review last 20 tickets, look for pattern | Retrain model, add examples |
| Response drafts missing | Check Claude API working? | Verify API key, check logs |
| KB search returning junk | Test search manually | Reindex KB, check vector DB |
| Agents not using AI drafts | Ask agents directly | Train again, adjust workflow if needed |
| System slow | Check API rate limits | Implement caching, add rate limiting |

---

## Support Contacts

- **Claude API issues:** support@anthropic.com
- **Zendesk integration:** support@zendesk.com
- **Pinecone KB:** support@pinecone.io
- **Your team:** Slack #ai-workflows

---

## Next Steps

1. ✅ Complete Phase 1-3 (Setup infrastructure & KB)
2. ✅ Complete Phase 4 (Deploy triage service)
3. ✅ Complete Phase 5 (Train team)
4. ✅ Start Phase 6 (Soft launch)
5. ✅ Go live with Phase 7

**Estimated timeline:** 2-3 hours setup + 1 week soft launch = Live in ~10 days

---

## Success Metrics (After 1 Month)

Your goal:

| Metric | Target | How to Measure |
|---|---|---|
| Triage Accuracy | >92% | Manual audit of 100 tickets |
| AI Draft Adoption | >80% | % of responses using AI draft |
| First-Response Time | <30 min | Average time to agent's first response |
| Response Quality | >85% | CSAT score on AI-assisted responses |
| Agent Satisfaction | >4/5 | Agent survey: "Does AI help?" |
| Time Saved | 15 min/ticket | Agent feedback on time savings |

---

**Ready to launch?** Start with Phase 1!