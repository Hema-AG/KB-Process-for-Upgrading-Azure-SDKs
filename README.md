# KB-Process-for-Upgrading-Azure-SDKs
Status: ✅ Active | Last Updated: 2024-05-24 | Owner: AI-ML Team

##1. Objective
This document provides a standardized, step-by-step procedure for safely upgrading Azure SDK dependencies across all AG projects. It is designed to minimize risk, ensure thorough testing, and serve as a reusable template for future upgrades.

##2. Scope
This process applies to all Python-based services at AG that utilize Azure SDK client libraries (e.g., azure-storage-blob, azure-identity, azure-keyvault-secrets).

##3. Pre-Requisites
- Access to the project's Git repository.
- A working Docker environment (matching our production setup).
- Access to project repo in ADO.
- A existing test suite with coverage for the Azure-integrated functionalities.
- Read access to the [Azure SDK for Python GitHub repositories](https://github.com/Azure/azure-sdk-for-python).

