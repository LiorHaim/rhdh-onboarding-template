# Onboarding Existing Services to RHDH

This repository contains a **Backstage Software Template** designed to solve the "Day 2" operational challenge: bringing existing, air-gapped workloads into Red Hat Developer Hub (RHDH) without disruption.

## 🎯 Goal

Our primary goal is to **simplify developer onboarding**. Developers already have services running on OpenShift, code in GitLab, and tracking in Jira. We don't want to change their workflow; we just want to surface their services in RHDH.

This template provides a seamless "wizard" experience that:
1.  **Respects Ownership**: Developers select only repositories they own.
2.  **Automates Metadata**: Generates a standardized `catalog-info.yaml`.
3.  **Prevents Errors**: Validates inputs (like K8s naming) before submission.
4.  **Connects the Dots**: Automatically links the service to:
    *   **GitLab** (Source Code)
    *   **OpenShift** (Workloads)
    *   **Jira** (Issues)
    *   **JFrog** (Artifacts)
    *   **ArgoCD** (GitOps)

## ✨ Features & User Experience

### 1. "Bring Your Own Repo"
Instead of scaffolding new code, this template uses the `RepoUrlPicker` to let developers select an *existing* GitLab repository.
- **Security**: It uses the user's own token to list only repositories they have permission to access.
- **Convenience**: No copy-pasting URLs manually.

### 2. Smart Defaults
We reduce friction by calculating defaults where possible:
- **Kubernetes ID**: Defaults to the Component Name if left empty (since they usually match).
- **Lifecycle**: Defaults to `production`.
- **Merge Request**: The change is proposed via a Merge Request (created by the RHDH bot), allowing the team to review the metadata before it becomes "standard".

### 3. Conditional Integrations
The template is flexible. It does not force every tool on every user.
- If a team doesn't use ArgoCD, they leave it blank.
- The resulting `catalog-info.yaml` is clean, containing *only* the annotations for the tools they actually use.

## 📂 Repository Structure

```
.
├── template.yaml            # The main Scaffolder template definition
└── skeleton/
    └── catalog-info.yaml    # The component definition file (Jinja2 template)
```

## 🛠 Usage Flow

1.  **Developer Action**: Logs into RHDH, clicks **"Onboard Existing Service"**.
2.  **Selection**: Picks their repo and fills in the integration "blanks" (Jira key, Argo app name, etc.).
3.  **Automation**: RHDH generates a Pull Request with a perfectly formatted `catalog-info.yaml`.
4.  **Completion**: The developer merges the PR. RHDH Auto-Discovery picks it up, and the service appears in the Catalog with all plugins (CI/CD, Kubernetes, etc.) lighting up immediately.
