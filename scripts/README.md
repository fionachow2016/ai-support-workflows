# Scripts Directory

This folder contains helper scripts and templates for implementation.

**Note:** These are templates/examples. Customize them for your environment before using.

---

## Available Scripts

### 1. `setup-environment.sh`
**Purpose:** Set up API keys and environment variables  
**Language:** Bash  
**Usage:** `bash setup-environment.sh`  
**Requires:** API keys for Claude, Zendesk, Pinecone

### 2. `scrape-shipstation-kb.py`
**Purpose:** Scrape ShipStation documentation and index in vector database  
**Language:** Python  
**Usage:** `python scrape-shipstation-kb.py`  
**Requires:** Pinecone API key

### 3. `test-triage-accuracy.py`
**Purpose:** Test triage accuracy on sample tickets  
**Language:** Python  
**Usage:** `python test-triage-accuracy.py --tickets 100`  
**Output:** Accuracy report, misclassified tickets

### 4. `deploy-to-production.sh`
**Purpose:** Deploy triage service to production  
**Language:** Bash  
**Usage:** `bash deploy-to-production.sh`  
**Requires:** Docker, cloud credentials (GCP/AWS)

---

## Before Running Scripts

1. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

2. **Set up environment:**
   ```bash
   cp .env.example .env
   # Edit .env with your API keys
   ```

3. **Test permissions:**
   ```bash
   # Can you access Zendesk API?
   curl -X GET https://[subdomain].zendesk.com/api/v2/users/me.json \
     -H "Authorization: Bearer $ZENDESK_API_TOKEN"
   ```

---

## Troubleshooting Scripts

### Script fails with "API key not found"
- Check `.env` file exists
- Verify API keys are set
- Run: `echo $ANTHROPIC_API_KEY` (should output key)

### KB scraping is slow
- Normal: Takes 2-3 minutes for 287 articles
- Check internet connection
- Verify Pinecone API working

### Triage test shows low accuracy
- May need to retrain model with more examples
- Review misclassified tickets for patterns
- See: [Implementation Guide](../docs/IMPLEMENTATION_GUIDE.md)

---

## Contributing Scripts

To contribute a script:

1. **Write it** with clear comments
2. **Add usage example** at the top
3. **Document inputs/outputs**
4. **Include error handling**
5. **Test thoroughly** before submitting
6. **Submit PR** with description

Example:
```python
#!/usr/bin/env python3
"""
Script: test-accuracy.py
Purpose: Test triage accuracy on sample tickets
Usage: python test-accuracy.py --tickets 100
Output: accuracy_report.json
"""

import argparse
import json

def main():
    # Your code here
    pass

if __name__ == "__main__":
    main()
```

---

## Security

**Never commit:**
- `.env` files with real API keys
- Credentials or passwords
- Customer data or sample tickets with PII
- Private deployment configurations

Use `.gitignore` to exclude these.

---

## Questions?

See the main [README](../README.md) or ask in Slack #ai-workflows
