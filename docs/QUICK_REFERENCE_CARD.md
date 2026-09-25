# AI Support Workflow: Quick Reference Card

**Print this and keep at your desk!**

---

## 🎯 The 3 Workflows

### 1️⃣ TICKET TRIAGE (Automatic)
```
Ticket arrives → AI reads it → Classifies by:
  • Category: Shipping, Billing, Integration, Account, Technical
  • Urgency: Critical, High, Medium, Low
  • Sentiment: Frustrated, Neutral, Positive
→ Auto-routes to right queue
```
**Your role:** Review tags. If wrong, edit and correct.

---

### 2️⃣ RESPONSE DRAFTING (AI + You)
```
You open ticket → AI shows:
  ✓ Draft response (ready to review)
  ✓ Top 3 KB articles (relevant docs)
  
You decide:
  ✅ Send as-is (if perfect)
  ✏️  Customize & send (most common)
  ✗ Reject & rewrite (if wrong)
```
**Time saved:** ~15 min per ticket

---

### 3️⃣ KB SEARCH (Automatic, then Manual if Needed)
```
AI automatically searches KB and includes top 3 articles
↓
If articles are wrong → You manually search
↓
Include better articles in response
```

---

## 📋 Triage Categories Quick Ref

| Category | Examples | Action |
|---|---|---|
| 🚚 **Shipping** | Tracking down, delayed, carrier issues | Check carrier status, provide link |
| 💳 **Billing** | Refund, payment failed, invoice | Verify account, check transaction |
| 🔌 **Integration** | API, webhook, Shopify sync | Check logs, provide code example |
| 👤 **Account** | Password reset, permissions | Verify identity, adjust access |
| ⚙️ **Technical** | Error, bug, performance | Reproduce, check logs, escalate |

---

## ✍️ Response Drafting Checklist

**Before clicking "Send":**

- [ ] Read the customer ticket carefully
- [ ] Review AI draft — does it answer their question?
- [ ] Check KB articles — are they relevant? Links work?
- [ ] Tone match? (Upset customer needs empathy, not just info)
- [ ] Personal touch? (Add their name, order #, specifics)
- [ ] No over-promises? (Don't promise refunds without approval)
- [ ] Clear next steps? (What if this doesn't work?)

**Time target:** 3-5 minutes per ticket

---

## 🚨 When to Escalate (Don't Respond)

**DO NOT respond yet. Escalate to team lead:**

❌ Customer demanding refund (need approval)  
❌ Angry customer threatening legal action  
❌ Security/privacy concern  
❌ Feature/bug you don't understand  
❌ Multiple issues from same customer  

**How to escalate:**
```
Add PRIVATE COMMENT in Zendesk:
@TeamLead: [Explain situation]
Recommend: [What should we do?]
Urgent? [Yes/No]
```

---

## 🏷️ Response Tags (Use These)

After you respond, add tags so we can track:

- `#ai-sent-as-is` — AI draft was perfect, sent without changes
- `#ai-customized` — AI draft was good, I personalized it
- `#human-written` — I wrote from scratch (AI wasn't helpful)
- `#escalated` — Forwarded to team lead
- `#kb-manual-search` — AI articles weren't good, I searched manually

---

## 🔍 Manual KB Search

**If AI articles aren't relevant:**

1. Click "Search KB" in Zendesk
2. Type naturally: "Why isn't tracking updating?"
3. Scan results (top 3 usually best)
4. Replace bad articles in response

**Good search phrasing:**
- ✅ "How do I bulk import orders?"
- ✅ "Why is my API integration broken?"
- ✅ "What's the refund policy?"

**Bad phrasing:**
- ❌ "bulk import" (too short)
- ❌ "API broken" (too vague)

---

## ⏱️ Time Targets

| Task | Target | Your Target |
|---|---|---|
| Review triaged ticket | <30 sec | Read tags |
| Review AI draft + KB | <2 min | Skim draft, check articles |
| Customize response | <3 min | Add personal touch |
| **Total per ticket** | **3-5 min** | |

**If spending >10 min per ticket:** You're over-perfecting. Trust AI more.

---

## 💡 Pro Tips

1. **Trust the AI draft:** 70% of the time it's good. Customize, don't rewrite.

2. **Personalize the greeting:** Change "Hi there" → "Hi [Name]" (takes 5 sec)

3. **Include specifics:** Add order #, tracking #, customer context (takes 20 sec)

4. **Tag responses:** Always add tag so we track what works (takes 5 sec)

5. **Escalate early:** If unsure, escalate. Better safe than sorry.

6. **Report KB gaps:** See a missing article? Mention it to knowledge manager.

7. **Read the runbooks:** If this card doesn't answer your question, check:
   - Ticket Triage Runbook
   - Response Drafting Runbook
   - KB Search Runbook

---

## ❌ Common Mistakes to Avoid

| Mistake | Why It's Bad | Fix |
|---|---|---|
| Send AI draft without reviewing | Could include wrong info | Always review (30 sec) |
| Forget to tag response | Can't track AI effectiveness | Add tag after sending |
| Over-customize response | Wasting time on perfection | Aim for 5 min total |
| Promise refund without approval | Sets bad expectations | Always escalate first |
| Ignore upset customer tone | Sounds robotic, hurts satisfaction | Add empathy to response |
| Manual KB search every ticket | Wastes time | Trust AI search 90% of time |

---

## 📞 Need Help?

| Question | Answer |
|---|---|
| How do I triage a ticket? | See: Ticket Triage Runbook |
| How do I review AI draft? | See: Response Drafting Runbook |
| How do I search KB manually? | See: KB Search Runbook |
| What if AI classified it wrong? | Edit tags in Zendesk |
| What if KB articles are bad? | Ping @knowledge-manager |
| What if customer is upset? | Escalate to @team-lead |
| AI not working? | Slack: #ai-workflows |

---

## 🎯 Daily Workflow

```
Open Zendesk
    ↓
See tickets in MY QUEUE (already triaged + drafted)
    ↓
FOR EACH TICKET:
  1. Read ticket (30 sec)
  2. Review AI draft + KB articles (2 min)
  3. Customize if needed (2 min)
  4. Quality check (1 min)
  5. Send & tag (1 min)
    ↓
TOTAL: 3-5 min per ticket
```

---

## 📊 What We're Tracking

Your team lead monitors:
- **Triage accuracy:** Are tickets classified right?
- **Response quality:** Are customers satisfied?
- **Time per ticket:** Are you becoming faster?
- **AI adoption rate:** Are you using AI or rejecting it?

**Why it matters:** Shows what's working, where to improve.

---

## 🚀 Remember

> **AI is here to help you work faster, not replace you.**
>
> You're the expert. AI handles the repetitive parts (search, draft).  
> You handle the human parts (empathy, judgment, customer relationship).
>
> Always review before sending. Your judgment > AI judgment.

---

**Questions?** Ask in Slack #ai-workflows or ping your team lead.

**More details?** Read the full runbooks on Confluence.

---

### Version: 1.0
### Last Updated: 2026-09-25
### Print Date: ____________