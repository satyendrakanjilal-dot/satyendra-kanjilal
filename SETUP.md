# How to Set Up This GitHub Portfolio

## Step 1: Create the Special Profile Repository

GitHub has a hidden feature: if you create a repo with the **exact same name as your GitHub username**, its `README.md` appears on your public profile page — automatically.

1. Go to [github.com/new](https://github.com/new)
2. Set the repository name to your exact GitHub username (e.g., `satyendra-kanjilal`)
3. Make it **Public**
4. Do NOT initialize with a README (you'll push your own)
5. Click **Create repository**

## Step 2: Push This Repo

If you have Git installed:

```bash
# Clone or initialize
git init
git add .
git commit -m "Initial portfolio: case studies, frameworks, and architecture docs"

# Connect to GitHub (replace with your actual GitHub username)
git remote add origin https://github.com/YOUR_USERNAME/YOUR_USERNAME.git
git branch -M main
git push -u origin main
```

## Step 3: Update Your LinkedIn

Add your GitHub profile URL to your LinkedIn:
- Edit your LinkedIn profile
- Under "Contact info" → add website → paste your GitHub profile URL
- Or add it directly in the "About" section

## Step 4: Keep This Living

This repo is most valuable when it's clearly active. Consider:

- **Monthly**: Update the "Currently Exploring" section in README.md
- **Quarterly**: Add a new case study or framework based on recent work
- **On major projects**: Document them within 2 weeks while fresh
- **On job hunt mode**: Pin this repo to the top of your GitHub profile

## Step 5: Add a Profile Photo

If you don't have a GitHub profile photo, add one that matches your LinkedIn. Consistency matters when recruiters are cross-referencing.

---

## File Structure Reference

```
satyendra-kanjilal/
├── README.md                          ← Your profile page (auto-shows on GitHub)
├── RESUME.md                          ← Metric-enhanced markdown resume
├── SETUP.md                           ← This file (can delete after setup)
├── case-studies/
│   ├── pxm-governance-at-scale.md    ← PXM governance model at Whirlpool
│   ├── sap-ccv2-migration.md         ← SAP Commerce Cloud migration
│   └── genai-content-ops.md          ← GenAI for content operations
├── frameworks/
│   ├── digital-shelf-readiness.md    ← Digital shelf readiness scoring model
│   └── devsecops-maturity-commerce.md ← DevSecOps maturity for eCommerce
└── architecture/
    ├── enterprise-pxm-ecosystem.md   ← PXM ecosystem architecture
    └── b2b-b2c-commerce-architecture.md ← Dual-channel commerce platform
```
