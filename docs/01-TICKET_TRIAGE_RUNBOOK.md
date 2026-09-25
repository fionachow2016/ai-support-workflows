# Ticket Triage & Routing Runbook

**Last Updated:** 2026-09-25  
**Audience:** Support Agents, Team Leads  
**Time to Read:** 5 minutes

---

## Overview

The AI triage system automatically classifies incoming support tickets and routes them to the correct team queue. This runbook explains what's happening behind the scenes and how to handle edge cases.

### What the AI Does Automatically

When a ticket arrives in Zendesk:
1. **Reads the ticket** (subject + first message)
2. **Classifies by category** (Shipping, Billing, Integration, Account, Technical)
3. **Scores urgency** (Critical, High, Medium, Low)
4. **Detects sentiment** (Frustrated, Neutral, Positive)
5. **Tags the ticket** and routes to correct queue
6. **Prepares suggested response** (see Response Drafting runbook)

**Time taken:** ~2 seconds per ticket (completely automated)

---

## Triage Categories Explained

### 🚚 **Shipping Issues**
**When:** Tracking problems, delayed shipments, carrier issues, label generation  
**Route to:** SHIPPING_TEAM queue  
**Typical urgency:** HIGH (customer impact immediate)  
**Example tickets:**
- "My shipment hasn't updated in 5 days"
- "FedEx tracking shows wrong status"
- "Why is my bulk label printing not working?"

**Agent action:** Check carrier status, provide tracking link, escalate if carrier issue

---

### 💳 **Billing & Invoicing**
**When:** Refund requests, failed payments, subscription questions, invoice disputes  
**Route to:** BILLING_TEAM queue  
**Typical urgency:** MEDIUM (financial, but not time-critical)  
**Example tickets:**
- "Why was I charged twice?"
- "Can I get a refund for unused credits?"
- "My credit card keeps declining"

**Agent action:** Verify account, check transaction history, process refunds if valid

---

### 🔌 **Integration Help**
**When:** API questions, webhook issues, third-party integrations, Shopify/WooCommerce sync  
**Route to:** INTEGRATION_TEAM queue  
**Typical urgency:** HIGH (blocks customer workflow)  
**Example tickets:**
- "My Shopify integration keeps disconnecting"
- "Webhook isn't firing when orders sync"
- "How do I use the ShipStation API?"

**Agent action:** Check API logs, verify credentials, provide code samples

---

### 👤 **Account Management**
**When:** Password resets, permission issues, user access, team member invites  
**Route to:** ACCOUNT_TEAM queue  
**Typical urgency:** MEDIUM-HIGH (blocks user access)  
**Example tickets:**
- "I forgot my password"
- "Can I add a team member?"
- "Why can't I access shipping reports?"

**Agent action:** Verify identity, reset password, adjust permissions

---

### ⚙️ **Technical/Troubleshooting**
**When:** System errors, feature bugs, performance issues, dashboard not loading  
**Route to:** TECHNICAL_TEAM queue  
**Typical urgency:** VARIES (could be critical outage or minor bug)  
**Example tickets:**
- "Dashboard is slow"
- "Getting error 500 when importing orders"
- "Mobile app crashes on login"

**Agent action:** Reproduce issue, check logs, create bug ticket if needed

---

## Urgency Levels

| Level | Definition | Response SLA | Examples |
|-------|-----------|-------------|----------|
| **CRITICAL** | System down, major data loss, revenue impact | 15 min | "All my orders disappeared", "Can't send any shipments" |
| **HIGH** | Significant business impact, customer upset | 1 hour | "Shipment delayed 5+ days", "API integration broken" |
| **MEDIUM** | Normal business problem, standard fix | 4 hours | "How do I enable bulk printing?", "Refund request" |
| **LOW** | General inquiry, nice-to-have feature | 24 hours | "What's the best way to...?", "Can you explain...?" |

---

## What to Do When AI Gets It Wrong

### Scenario: Ticket is Misclassified

**You see:** Ticket routed to BILLING but it's actually a shipping issue

**Action:**
1. Click the **Edit Tags** button in Zendesk
2. Remove incorrect tag (e.g., `#billing`)
3. Add correct tag (e.g., `#shipping`)
4. Move to correct queue
5. Add comment: "Reclassified - was billing, actually shipping"

**Why this matters:** Each correction teaches the AI to classify better next time.

### Scenario: Urgency is Wrong

**You see:** Low priority ticket that should be HIGH (upset customer, business impact)

**Action:**
1. Click **Change Priority** in Zendesk
2. Select correct urgency level
3. Move to appropriate queue
4. Add comment: "Urgency adjusted to HIGH - customer escalation risk"

**Note:** AI usually gets urgency right. If this happens frequently, notify your team lead.

---

## Routing Rules Reference

Use this table if you need to manually route a ticket:

```
CATEGORY + URGENCY = QUEUE + SLA

Shipping + Critical  → CRITICAL_SHIPPING    → 15 min response
Shipping + High      → SHIPPING             → 1 hour response
Billing + High       → BILLING              → 1 hour response
Billing + Medium     → BILLING              → 4 hour response
Integration + High   → INTEGRATION          → 1 hour response
Account + High       → ACCOUNT              → 1 hour response
Technical + Critical → CRITICAL_TECHNICAL   → 15 min response
General + Low        → GENERAL_QUEUE        → 24 hour response
```

---

## Handling Ambiguous Tickets

Sometimes tickets don't fit neatly into one category. Here's how to handle them:

### Multi-Category Ticket
**Example:** "My shipment didn't arrive AND I was charged incorrectly"

**Action:**
- Tag with **primary issue first** (e.g., `#shipping`)
- Add secondary tag (e.g., `#billing`)
- Route to primary team
- Add note: "Also involves billing - may need escalation"
- Secondary team gets notified and follows up if needed

### Cross-Team Issue
**Example:** "Integration broke and now I have duplicate orders"

**Action:**
- Use judgment: Which impacts customer more?
- If data integrity issue → TECHNICAL_TEAM (critical)
- If just API sync → INTEGRATION_TEAM
- Tag both and escalate to team lead if unsure

---

## Metrics to Track

Your team lead monitors these weekly:

| Metric | Target | What It Means |
|--------|--------|---------------|
| **Triage Accuracy** | >92% | % of tickets correctly classified |
| **First-Response Time** | <30 min | How fast AI + agent start responding |
| **Reclassification Rate** | <8% | % of tickets manually corrected by agents |
| **Queue Balance** | Relative | Even distribution across team |

---

## Quick Reference: AI Triage Workflow

```
📥 Ticket arrives in Zendesk
    ↓
🤖 AI reads ticket text
    ↓
🏷️  AI assigns tags:
    - Category (#shipping, #billing, etc.)
    - Urgency (#critical, #high, #medium, #low)
    - Sentiment (#frustrated, #neutral, #positive)
    ↓
🎯 Ticket auto-routes to queue
    ↓
✍️  AI prepares draft response (see Response Drafting runbook)
    ↓
👤 Agent receives triaged ticket with:
    - Tags clearly visible
    - Suggested response template
    - Top 3 KB articles
    ↓
✅ Agent reviews everything before responding
```

---

## Troubleshooting

### "Tickets aren't getting routed to my queue"
- **Check:** Are your team members assigned to the queue in Zendesk settings?
- **Check:** Is the queue enabled in automation rules?
- **Action:** Notify your team lead

### "AI keeps misclassifying one type of ticket"
- **Document:** 5-10 examples of the misclassification
- **Notify:** Your team lead with examples
- **Action:** Your team lead will retrain the model with these examples

### "Response time is slow for some tickets"
- **Normal:** Takes ~2 sec to classify, then AI needs to search KB (another 2-3 sec)
- **Total:** Usually <5 seconds from ticket arrival to draft response ready
- **If slower:** Check Zendesk system status and Claude API status

---

## FAQ

**Q: Does the AI make mistakes?**  
A: Yes, occasionally. When it does, you correct it. That correction trains the model.

**Q: Can I override the AI's classification?**  
A: Yes, always. Your judgment > AI judgment. Correct it and it learns.

**Q: What if a ticket needs multiple queues?**  
A: Tag with multiple categories, route to primary team, add note for secondary team.

**Q: How does AI know about our products?**  
A: It's trained on ShipStation's public docs + your company KB (articles and FAQs).

---

**Need help?** Slack #ai-workflows or mention @support-ops-team