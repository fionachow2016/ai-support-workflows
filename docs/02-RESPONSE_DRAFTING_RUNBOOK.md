# Response Drafting & Quality Assurance Runbook

**Last Updated:** 2026-09-25  
**Audience:** Support Agents (Tier 1 & 2)  
**Time to Read:** 7 minutes

---

## Overview

For every triaged ticket, the AI generates a first-draft response. Your job is to review it, customize it for the customer, and send it out. This runbook shows you how to do that efficiently and maintain quality standards.

### The Draft Response Workflow

```
Agent receives triaged ticket
            ↓
AI has already fetched top 3 KB articles (in the draft)
            ↓
AI has written first-draft response (ready to review)
            ↓
YOU review the draft and decide:
  - Send as-is (if perfect)
  - Customize & send (most common)
  - Reject & rewrite (if AI missed the mark)
            ↓
Send to customer
```

**Time saved per ticket:** ~15 minutes (no searching KB, no starting from scratch)

---

## Understanding AI Draft Responses

### What Makes a Good AI Draft

✅ **Acknowledges** the customer's problem  
✅ **Provides relevant** KB links and solutions  
✅ **Offers next steps** if the customer needs more help  
✅ **Uses professional but friendly** tone  
✅ **Includes company details** (hours, contact info if needed)  

### What AI Doesn't Do Well

❌ **Personal context** - AI doesn't know your company's customer relationships  
❌ **Exceptions** - AI won't offer a refund or exception without your review  
❌ **Escalation decisions** - AI suggests, you decide  
❌ **Sensitive situations** - Upset customers need human empathy, not just info  

---

## Step-by-Step: Reviewing a Draft Response

### Step 1: Read the Customer Ticket
- Understand what they're really asking
- Note their **tone** (frustrated, polite, urgent)
- Identify any **special context** (VIP customer, long-time account, repeat issue)

### Step 2: Review the AI Draft
The draft appears in a "Suggested Response" section in Zendesk. It includes:
- **Greeting** (usually generic, personalize if needed)
- **Acknowledgment** (AI shows it understands the problem)
- **Solution** (Usually 2-3 KB articles or steps)
- **Next steps** (What if their problem isn't solved?)
- **Signature** (Company info)

### Step 3: Check the KB Articles
Scroll down to see the 3 KB articles AI suggested:

```
[1] "How to track your ShipStation order" (92% match)
    → Is this actually relevant? Good match.

[2] "Why shipments are delayed" (87% match)
    → Is this what the customer asked about? Check.

[3] "Carrier updates guide" (81% match)
    → Is this helpful? Yes.
```

**Ask yourself:** Do these articles answer the customer's question?
- **If yes** → Keep them in the response
- **If no** → Search KB yourself and replace with better articles
- **If partial** → Reorder to put best answer first

### Step 4: Decide: Send, Customize, or Rewrite

#### Option A: Send As-Is (Rare)
✅ **Use when:** AI draft is perfect, tone matches customer, all info is correct

**Action:**
1. Click **Send** in Zendesk
2. Tag as `#ai-generated-sent-as-is`
3. Move on to next ticket

**Reality:** Only ~20% of AI drafts are sent without changes. That's normal!

---

#### Option B: Customize & Send (Most Common - 70%)
✅ **Use when:** Draft is good but needs your personal touch

**Common customizations:**

| Situation | What to Change | Example |
|-----------|---|---|
| Generic greeting | Replace with customer name | "Hi John," instead of "Hi there," |
| Missing reference to their specific issue | Add details they mentioned | Include their order #, carrier name, etc. |
| Neutral tone but customer is upset | Add empathy | "I completely understand your frustration with..." |
| Generic KB link | Add context about which step they should follow | "Start at Step 3 in this guide..." |
| Missing company details | Add SLA, contact info if relevant | "We'll check with FedEx and follow up within 2 hours" |
| No escalation when needed | Offer next step | "If this doesn't work, reply and I'll escalate to our technical team" |

**Example: Before & After**

```
BEFORE (AI Draft):
---
Hi there,

Thanks for reaching out about your shipment delay. We understand tracking 
can be confusing. Here's our guide on why shipments are delayed:
[Link]

Let us know if you need anything else.

Thanks,
ShipStation Support Team

---

AFTER (Agent Customized):
---
Hi John,

I completely understand how frustrating it is when tracking doesn't update 
for 5 days. I'd be upset too. For your shipment #SS-12845 (FedEx), here's 
what's usually happening:

FedEx sometimes has a 24-48 hour sync delay with ShipStation. Here's our 
guide with steps to check directly with FedEx: [Better Link]

I've already checked on our end—shipment shows as "In Transit" on FedEx's 
system. If it still hasn't updated by tomorrow morning, reply and I'll 
escalate this to our operations team for a manual trace.

You'll hear from me either way by 5pm tomorrow.

Best,
Sarah (ShipStation Support)
---
```

**Action:**
1. Click **Edit** on the AI draft
2. Make your changes
3. Click **Send**
4. Tag as `#ai-customized`

---

#### Option C: Rewrite (Uncommon - 10%)
❌ **Use when:** AI misunderstood the issue or gave bad advice

**When to rewrite:**
- AI classified ticket wrong (routing to wrong team)
- AI suggested wrong KB article
- Customer asked for something AI can't help with (e.g., feature request)
- Situation requires judgment AI can't make (e.g., exception approval)

**Action:**
1. Click **Clear** to dismiss AI draft
2. Write your own response
3. Manually search KB if needed (see Knowledge Base runbook)
4. Send
5. Tag as `#human-written` so we can learn why AI failed

---

## Quality Checklist Before Sending

Before clicking **Send**, verify:

- [ ] **Tone matches customer.** Are they upset? Is response empathetic?
- [ ] **Accuracy.** Does the response correctly answer their question?
- [ ] **Links work.** Click any KB links to verify they still work.
- [ ] **No over-promises.** Did you promise something you can't deliver?
- [ ] **Specific details.** Did you include their order #, name, or specifics?
- [ ] **Clear next steps.** Does customer know what to do if this doesn't work?
- [ ] **Professional but friendly.** Does it sound like the company?

---

## Handling Different Scenarios

### Scenario 1: Customer is Very Upset
**Tone:** Frustrated, angry, threatening to leave

**What AI draft misses:** Emotional validation  

**Your action:**
- Lead with empathy: "I completely understand your frustration..."
- Acknowledge the failure: "You're right—this should have shipped by now"
- Take ownership: "Let me help fix this"
- Offer clear timeline: "I'll personally follow up by 2pm today"

**Example:**
```
Hi [Customer],

I just read through your message and I completely understand your 
frustration. You ordered this 10 days ago and it still hasn't shipped—
that's unacceptable, and I'm sorry we let you down.

Let me fix this right now:
1. [Specific action you'll take]
2. [Timeline]
3. [How you'll follow up]

I'll email you personally by [TIME] with an update.

Best,
[Your name]
```

---

### Scenario 2: Technical Question We Can't Answer
**Customer:** "Why is my API rate limit 1000/min instead of 5000/min?"

**What AI draft does:** Links to generic rate limit docs

**Your action:**
- Be honest: "This is a great technical question I need to escalate"
- Assign properly: Route to INTEGRATION_TEAM with context
- Set expectations: "You'll hear from our technical specialist within 2 hours"
- Don't make up answers

**Example:**
```
Great question! This is a technical configuration I need to check with 
our team. I'm escalating this to our integration specialist who can 
access your account settings.

You'll hear from them within 2 hours.

Thanks for your patience,
[Your name]
```

---

### Scenario 3: Refund/Exception Request
**Customer:** "Can I get a refund for unused credits?"

**What AI draft does:** Links to refund policy

**Your action:**
- **NEVER** approve a refund in your response without manager approval
- Instead: "Let me check with my manager and get back to you within 2 hours"
- Escalate to team lead in a comment (private)
- Wait for approval before responding

**Example:**
```
Thanks for asking about a refund on your unused credits. Let me check 
with my manager to see what options we have for your account.

I'll reply with an answer by end of business today.

Thanks,
[Your name]

---
[PRIVATE COMMENT TO TEAM LEAD]
@TeamLead: Customer requested refund for $250 in unused credits. Account 
is 3 years old, strong customer. Recommend approval. OK to approve?
```

---

### Scenario 4: Feature Request
**Customer:** "Can you add bulk CSV import?"

**What AI draft does:** Explains current process

**Your action:**
- Don't promise features
- Validate their need: "Great idea—that would save you time"
- Direct to feedback channel: "I'm forwarding this to product team"
- Keep them updated: "You can track this in our public roadmap"

**Example:**
```
That's a great suggestion! CSV bulk import would definitely save you 
time. I'm forwarding this to our product team.

In the meantime, you can:
1. [Current workaround]
2. [Alternative approach]

Plus, you can vote on this feature idea in our public roadmap here: [Link]

Thanks for the feedback!
```

---

## Response Quality Metrics

Your team lead tracks these metrics:

| Metric | Target | What It Means |
|--------|--------|---------------|
| **Customer Satisfaction (CSAT)** | >85% | % of customers satisfied with response |
| **First-Contact Resolution** | >75% | % resolved without follow-up |
| **Response Accuracy** | >92% | % of responses that correctly answer question |
| **Avg Customization Time** | <5 min | How long agents spend reviewing/customizing |
| **AI Draft Adoption Rate** | >80% | % of responses using AI draft (not rejecting) |

---

## Common Mistakes to Avoid

### ❌ Mistake 1: Sending AI Draft Without Reading
- **Problem:** AI sometimes gives outdated or incorrect info
- **Solution:** Always review KB articles for accuracy
- **Check:** Does the KB article match customer's ShipStation version?

### ❌ Mistake 2: Over-Personalizing
- **Problem:** Spending 30 min customizing a simple response
- **Solution:** Use AI draft for 90% of work, add personal touch for last 10%
- **Benchmark:** Should spend <5 min per response

### ❌ Mistake 3: Forgetting to Tag Responses
- **Problem:** We can't track which responses were AI-assisted
- **Solution:** Always add tag: `#ai-customized`, `#ai-sent-as-is`, or `#human-written`
- **Why matters:** Helps us measure AI effectiveness

### ❌ Mistake 4: Making Promises Without Authority
- **Problem:** "I'll refund you" or "I'll fix this in 2 hours"
- **Solution:** Check your authority first. Escalate if uncertain.
- **Safe language:** "Let me check and get back to you"

### ❌ Mistake 5: Ignoring Upset Customers
- **Problem:** AI draft treats all customers the same
- **Solution:** Upgrade tone for frustrated customers
- **Signs:** ALL CAPS, exclamation marks, words like "unacceptable", "disgusted"

---

## Quick Reference Card

```
WORKFLOW:
1. Read customer ticket
2. Review AI draft
3. Check KB articles (relevant?)
4. Decide: Send / Customize / Rewrite
5. Add personal touch if customizing
6. Quality check (tone, accuracy, promises)
7. Send & tag appropriately

TIME TARGET: 3-5 minutes per ticket

TAGS TO USE:
#ai-sent-as-is      → Draft was perfect
#ai-customized      → Draft was good, I personalized it
#human-written      → I wrote it from scratch
```

---

## Troubleshooting

### "AI draft is completely wrong"
- **Action:** Click Reject and rewrite
- **Notify:** Tag as `#human-written`
- **Report:** If this happens 3+ times on similar tickets, notify team lead

### "KB article link is broken"
- **Action:** Search KB yourself, replace with working link
- **Report:** Notify knowledge manager that article needs update

### "I'm spending too much time customizing"
- **Reality check:** If spending >10 min per response, you're over-perfecting
- **Solution:** Use the AI draft more, trust it more
- **Ask:** "Would customer notice the difference?" If no, send it.

### "Customer replied asking for exception (refund, waive fee)"
- **Action:** Do NOT respond directly
- **Escalate:** Add private comment: "@TeamLead: Exception request. Awaiting approval."
- **Wait:** For manager approval before replying

---

## FAQ

**Q: What if the AI draft is way off?**  
A: That's fine! Reject it, write your own. This helps us improve. Just tag it `#human-written`.

**Q: Can I send the AI draft without reviewing?**  
A: Technically yes, but don't. Always review. It only takes 30 seconds and catches mistakes.

**Q: What if customer replies they weren't satisfied?**  
A: Escalate to team lead. We track these to improve AI responses.

**Q: How much should I personalize?**  
A: Enough to feel like a human wrote it, but not so much that you're spending 15 min per ticket.

**Q: What if I don't know what the customer is asking?**  
A: Escalate or rewrite. Say "Let me connect you with someone who specializes in this."

---

**Need help?** Slack #ai-workflows or ask your team lead