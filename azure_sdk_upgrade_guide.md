# The Upgrade Process: A Step-by-Step Guide

## Phase 1: Identify the Azure SDKs
1. Open your application in its container.
2. Run the following code in the terminal and you should get an output like this:

```bash
pip list | grep -i azure
```

**Example Output:**

```
azure-common           1.1.28
azure-core             1.35.0
azure-identity         1.17.1
azure-search-documents 11.6.0b4
azure-storage-blob     12.22.0
```

---

## Phase 2: Research Target Versions & Changelogs
For each package, find the latest stable (non-beta) version and read its changelog.

**Changelog Path Format:**
```
https://github.com/Azure/azure-sdk-for-python/tree/main/sdk/{service}/{package}/CHANGELOG.md
```

**Your Action:** Create a summary table in your PR description like this:

| Package                  | Current Version | Target Version | Breaking Changes?                 | Changelog Link |
|---------------------------|----------------|----------------|----------------------------------|----------------|
| azure-storage-blob        | 12.17.0        | 12.19.0        | No                               | [Link](https://github.com/Azure/azure-sdk-for-python/blob/main/sdk/storage/azure-storage-blob/CHANGELOG.md) |
| azure-search-documents    | 11.4.0b9       | 11.6.0         | Yes (facets → facet_results)      | [Link](https://github.com/Azure/azure-sdk-for-python/blob/main/sdk/search/azure-search-documents/CHANGELOG.md) |
| azure-identity            | 1.13.0         | 1.15.0         | No                               | [Link](https://github.com/Azure/azure-sdk-for-python/blob/main/sdk/identity/azure-identity/CHANGELOG.md) |

---

## Phase 3: Implementation

### Step 1: Create a Feature Branch
Isolate your work on a new branch.

```bash
git checkout main
git pull origin main
git checkout -b upgrade/azure-sdks-$(date +%Y%m%d) # e.g., upgrade/azure-sdks-20240524
```

### Step 2: Update `requirements.txt`
Pin the new, vetted versions. Avoid using `@latest`.

```bash
# Example changes inside requirements.txt:
# - azure-identity==1.13.0
# + azure-identity==1.15.0
# - azure-search-documents==11.4.0b9
# + azure-search-documents==11.6.0
```

### Step 3: Install Dependencies & Smoke Test
Rebuild the container and verify basic functionality.

```bash
# Rebuild with new dependencies
docker-compose down
docker-compose build
docker-compose up -d

# Run a basic smoke test to check for import errors
docker-compose exec app python -c "
import azure.identity
import azure.storage.blob
print('✓ All imports successful')
"
```

### Step 4: Run Targeted Tests
Test each upgraded service individually to isolate failures.

```bash
# Example: Test Blob Storage functionality
docker-compose exec app python -m pytest tests/test_doc_handlers.py -v

# Example: Test AI Search functionality
docker-compose exec app python -m pytest tests/test_ai_search.py -v
```

### Step 5: Fix Breaking Changes
Address issues identified during research and testing.

**Example Fix for `azure-search-documents`:**

```python
# BEFORE:
if result.facets:
    process_data(result.facets)

# AFTER:
if result.facet_results:  # Property renamed in v11.5.0
    process_data(result.facet_results)
```

Commit fixes as you go:

```bash
git add services/svc_ai_search.py
git commit -m "fix(ai_search): rename facets to facet_results for SDK v11.6.0"
```

---

## Phase 4: Validation & Merge

### Step 6: Run Full Test Suite
Execute the complete test suite to check for regressions.

```bash
docker-compose exec app python -m pytest tests/ -v --tb=short
```

### Step 7: Create a Pull Request (PR)
Push your branch and create a PR for review.

```bash
git push origin upgrade/azure-sdks-20240524
```

**PR Description Template:**

```markdown
## Upgrade Azure SDKs

### Summary
This PR upgrades Azure SDK packages to their latest stable versions to incorporate security patches, bug fixes, and performance improvements.

### Changes
- Updated `requirements.txt` with pinned versions.
- **Code Changes:** Fixed breaking change in `services/svc_ai_search.py` due to the `azure-search-documents` upgrade.

### Changelog Review
All upgrades were preceded by changelog review to identify breaking changes:
- **[azure-search-documents v11.5.0](https://github.com/Azure/azure-sdk-for-python/blob/main/sdk/search/azure-search-documents/CHANGELOG.md#1150-2023-10-10):** `search_results.facets` was renamed to `facet_results`.
- [azure-storage-blob v12.19.0](https://github.com/Azure/azure-sdk-for-python/blob/main/sdk/storage/azure-storage-blob/CHANGELOG.md): No breaking changes.
- [azure-identity v1.15.0](https://github.com/Azure/azure-sdk-for-python/blob/main/sdk/identity/azure-identity/CHANGELOG.md): No breaking changes.

### Testing
- [x] Full test suite passes (`pytest tests/ -v`).
- [x] Manual smoke test performed in development environment.
- [x] Changes validated against live Azure endpoints.

### Rollback Plan
Revert this PR and rebuild the container. The previous version of `requirements.txt` is known stable.
```

### Step 8: Merge and Deploy
After approval, merge the PR. Deploy through the standard CI/CD pipeline and monitor application logs closely for any unexpected behavior post-deployment.
