# Knowledge Base Search & Synthesis Runbook

**Last Updated:** 2026-09-25  
**Audience:** Support Agents, Knowledge Manager, Team Leads  
**Time to Read:** 6 minutes

---

## Overview

The AI system has indexed all ShipStation public documentation plus your company's internal knowledge base. When a ticket arrives, it automatically finds the 3 most relevant articles and includes them in the response draft. This runbook explains how to use, maintain, and improve the KB.

### What's in the KB

```
📚 Knowledge Base Contents:
├─ ShipStation Public Docs (287 articles)
│  ├─ Getting Started Guides
│  ├─ Shipping & Tracking
│  ├─ Integrations (Shopify, WooCommerce, BigCommerce, etc.)
│  ├─ API Reference
│  ├─ Troubleshooting
│  └─ Account Management
│
└─ Company Internal KB (maintained by you)
   ├─ Support Playbooks
   ├─ Escalation Procedures
   ├─ Company Policies
   ├─ Frequently Asked Questions
   └─ Edge Cases & Workarounds
```

**Total articles:** ~350 (287 public + 60+ internal)  
**Last updated:** Daily (ShipStation) + As needed (Internal)  
**Search method:** Semantic search (AI understands meaning, not just keywords)

---

## How KB Search Works

### The Process

```
1. Ticket arrives: "My tracking doesn't show up for 24 hours"

2. AI converts to search query:
   → Looks for articles about "tracking delays", "sync delays", "FedEx timing"

3. Searches entire KB for matches:
   → "Why shipments are delayed" (94% match)
   → "Tracking not updating in real time" (88% match)
   → "FedEx integration sync timing" (82% match)

4. Returns top 3 articles with:
   → Title
   → Link
   → Match confidence %
   → Relevant snippet

5. Agent receives results and includes in response draft
```

### Why "Semantic" Search is Better

| Old Way (Keyword) | New Way (Semantic AI) |
|---|---|
| Search "tracking" → finds exact word match | Search "where is my shipment?" → understands you mean tracking |
| Misses synonyms (status, location, whereabouts) | Finds all synonyms automatically |
| Can't understand context | Understands: customer frustrated, needs immediate info |
| Returns irrelevant results | Returns highest-confidence matches first |

---

## Using KB Search in Your Workflow

### Method 1: AI Automatically Finds Articles (Most Common)

**You see:** Draft response arrives with KB articles already included

```
Suggested Response
───────────────────
Hi Sarah,

Thanks for your question! Here are the most relevant articles:

[1] "How to track your ShipStation order" (92% match)
    https://shipstation.com/help/tracking
    
[2] "Why shipments take time to update" (87% match)
    https://shipstation.com/help/tracking-delays
    
[3] "Understanding carrier status updates" (79% match)
    https://shipstation.com/help/carrier-status
```

**Your action:**
- ✅ If articles are good: Keep them in response
- ❌ If articles aren't relevant: Delete and search manually (see Method 2)

---

### Method 2: Manually Search KB (When AI Picks Wrong Articles)

**You see:** AI included irrelevant articles, or customer asked something specific

**Steps:**

1. **Click "Search KB" button** in Zendesk
2. **Type your search:**
   - ✅ Type naturally: "How do I bulk import orders?"
   - ✅ Be specific: Include product name if relevant
   - ❌ Don't: Use keywords like "bulk import orders" (we use semantic search)

3. **Review results:** AI shows top 5-10 matches with confidence scores

   ```
   RESULTS:
   
   [1] "Bulk Order Import Guide" (96% match) ← This one
   [2] "Importing CSV Files" (91% match)
   [3] "Batch Processing Options" (85% match)
   [4] "Manual Order Entry" (72% match) ← Too low, ignore
   [5] "API Bulk Orders" (68% match)
   ```

4. **Select best 3** and add to response

**Tips for better searches:**
- "tracking issues" → Better phrased as "Why isn't my tracking updating?"
- "API" → Better phrased as "How do I use the ShipStation API?"
- "integration broken" → Better phrased as "Shopify sync not working"

---

## Maintaining the Knowledge Base

### For Knowledge Manager Role

#### Daily: Sync ShipStation Public Docs
- **Task:** Scraper automatically updates daily at 2am
- **Check:** Run daily test search to verify new articles indexed
- **Monitor:** If articles drop off index, investigate why

**Example commands:**
```bash
# Manual sync if needed
python scripts/sync_shipstation_kb.py

# Test search
curl -X POST http://api/kb-search \
  -d "query=tracking+not+updating"
```

#### Weekly: Review & Approve Internal KB Edits
- **Task:** Agents may submit KB improvements
- **Process:**
  1. Agent submits: "Tracking article is outdated - FedEx changed their API"
  2. Knowledge manager reviews
  3. If good: Approve and publish
  4. If wrong: Request revision
- **Metric:** Publish at least 2 KB improvements per week

#### Monthly: Analyze KB Gaps
- **Review:** Which tickets had NO good KB match?
- **Action:** Create new KB articles for gaps
- **Report:** Share "Top 5 KB gaps this month" with team lead

#### Quarterly: Prune Outdated Articles
- **Review:** Articles older than 6 months
- **Action:** Archive or update outdated content
- **Metric:** Keep KB fresh (>80% articles updated in last 6 months)

---

### For All Agents: Contribute to KB

#### Report KB Issues

**See a problem?** Make it visible:

```
In Zendesk ticket, add Private Comment:

@KnowledgeManager: 
The article "Bulk Shipping Labels" is outdated. 
Step 3 no longer exists in current UI. 
Customers are getting confused.

Link: [Article URL]
```

#### Suggest New Articles

**See a recurring question?** Document it:

```
@KnowledgeManager:
Customers keep asking "How do I contact my carrier directly from ShipStation?"
We don't have a good article for this.

Suggested article outline:
1. Which carriers support direct contact
2. How to find carrier contact info in ShipStation
3. What questions to ask carrier directly
```

#### Add to Internal KB

**Have a workaround or best practice?** Share it:

1. Go to: Confluence → ShipStation Support KB
2. Click **New Page**
3. Title: "[Your Team] - Topic Name"
   - Example: "Billing Team - How to Process Partial Refunds"
4. Follow template (see below)
5. Tag with your team name
6. Ping knowledge manager to review

---

## Internal KB Article Template

Use this template for any new articles you contribute:

```
# [Title: Clear, customer-friendly title]

## When to Use This Article
[When do customers ask this? What problem does it solve?]

## Quick Answer
[1-2 sentence answer for impatient customers]

## Step-by-Step Guide
1. [First step with screenshot if visual]
2. [Next step]
3. [Next step]

## Common Mistakes
- ❌ [Mistake 1]
- ❌ [Mistake 2]

## Related Articles
- [Link to article A]
- [Link to article B]

## Still Need Help?
[When to escalate, how to escalate]
```

**Example:**
```
# How to Issue a Partial Refund

## When to Use This Article
Customer wants refund for part of order (e.g., overpaid shipping)

## Quick Answer
You can't issue partial refunds through UI. 
Use the manual workaround below.

## Step-by-Step Guide
1. In customer account, find the order
2. Calculate refund amount
3. Process via [Internal tool] 
4. Note reason in order comments
5. Email customer with refund confirmation

## Common Mistakes
- ❌ Issuing full refund instead of partial
- ❌ Forgetting to update order notes

## Related Articles
- [Refund Policy](link)
- [Handling Billing Disputes](link)

## Still Need Help?
Escalate to billing manager if customer demands credit
```

---

## KB Search Quality Metrics

Your team tracks these metrics:

| Metric | Target | What It Means |
|--------|--------|---------------|
| **KB Match Success** | >90% | % of AI searches returning relevant articles |
| **KB Coverage** | >85% | % of support questions with KB answer |
| **Article Freshness** | >80% | % of articles updated in last 6 months |
| **Manual Searches** | <10% | % of tickets requiring manual KB search |
| **KB Contribution Rate** | 2/week | New/updated articles per week |

---

## Troubleshooting

### "AI suggests irrelevant KB articles"

**Scenario:** Customer asks about "Refunds" but AI returns articles about "Returns"

**Why:** KB might have gap or confusing naming

**What to do:**
1. Report the mismatch (ping knowledge manager)
2. Do manual search and use correct article
3. Tag as `#kb-search-failed-manual-override`

**Knowledge manager action:**
- Create new article: "Refund Policy" (if missing)
- Or improve existing article title/tags

---

### "Searched but found no good articles"

**Scenario:** Customer asks: "How do I sync with TikTok shop?" but no articles exist

**What to do:**
1. Admit the gap: "We don't have a guide for this yet"
2. Escalate: "Let me connect you with someone who can help"
3. Report to knowledge manager: "Feature request: TikTok integration article"

**Knowledge manager action:**
- Determine if TikTok is supported
- If yes: Create article
- If no: Add to FAQ

---

### "Old article is still showing up in searches"

**Scenario:** Article is about "Old Dashboard UI" but we updated to "New Dashboard UI"

**What to do:**
1. Report to knowledge manager
2. Search again with different terms
3. Use newer article instead

**Knowledge manager action:**
- Review and archive old article
- Ensure new article is indexed and discoverable

---

## FAQ

**Q: Can I add my own KB article?**  
A: Yes! Follow the template, tag it with your team name, and ping knowledge manager for approval.

**Q: What if customer needs info not in KB?**  
A: Escalate. "Let me connect you with someone who specializes in this." We'll use that as input to create new KB article.

**Q: How often is KB updated?**  
A: ShipStation public docs sync daily. Internal KB updated as needed. New articles typically indexed within 24 hours.

**Q: Why is an old article still showing in search?**  
A: KB caching. Can take up to 24 hours to remove archived articles. Report if urgent.

**Q: Can customers see internal KB?**  
A: No. Only agents see internal KB. Customers see public ShipStation docs.

---

## Quick Reference: KB Workflow

```
📥 Ticket arrives
    ↓
🤖 AI searches KB automatically
    ↓
📄 Returns top 3 articles
    ↓
👤 Agent reviews articles
    
    Branch 1: ✅ Good match
    → Include in response
    
    Branch 2: ❌ Bad match
    → Manual search
    → Select correct articles
    → Include in response
    
    Branch 3: 📭 No match found
    → Escalate
    → Report KB gap
    → KB manager creates article
    
    ↓
✍️ Response sent with KB links
```

---

## Integration with Other Workflows

### Ticket Triage
- KB search happens AFTER triage
- Uses triage category to find relevant articles
- "Shipping" tickets get shipping KB articles, etc.

### Response Drafting
- KB articles auto-included in draft
- Agent can replace if needed
- All KB links tracked in Zendesk

### Analytics
- Track which KB articles are used most
- "Tracking issues" article used 450x/month
- "Bulk label printing" article used 120x/month
- Use data to prioritize which articles to improve

---

**Need help?** Slack #ai-workflows or ping @knowledge-manager