# FAQ: Azure SDK Upgrade Process

## 1. What if the changelog is huge and hard to read?
Focus on the **"Breaking Changes"** headers first.  
Use GitHub's **blame view** on the `CHANGELOG.md` file to see exactly which commits introduced changes in the versions you care about.

---

## 2. A package doesn't have a clear changelog. What then?
Prefer packages under the **Azure/azure-sdk-for-python** umbrella.  
If a third-party package lacks documentation, consider it a **risk** and discuss replacing it with a first-party SDK with the team.

---

## 3. How do we handle upgrades for other languages (e.g., .NET, TypeScript)?
This process is **language-agnostic**.  
The same principles apply: isolate, research changelogs, test, fix.  

- **.NET** → Use **NuGet**.  
- **TypeScript** → Use **npm**.  

We should create **separate KB docs** for those standard toolchains.

---

# Template for New Projects

Copy this section into your new project's `CONTRIBUTING.md` or `docs/` folder.

## To Upgrade Dependencies:
- **Follow the Standard Process:** This upgrade is governed by our company-wide KB guide:  
  [Link to this KB Article in the company-knowledge-base repo]

- **Use the PR Template:** Copy the PR template from section **3.2** of the KB guide.

- **Mandatory Changelog Review:** Linking to the specific changelog sections that justify the changes is a required part of the PR review process.

---

# Document Info
- **Document ID:** KB-ENG-005  
- **Changelog:**
  - 2024-05-24: Created by [Your Name/Team]. Approved by [Tech Lead Name].
