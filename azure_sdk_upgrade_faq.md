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

---

# Document Info
- **Document ID:** KB-AZSDKUPDT-001  
- **Changelog:**
  - 2024-05-24: Created by Hema Varssini Segar - AI-ML Team. Approved by Nuzli Anas.
