# Architecture decisions

This document records the technical decisions that constrain the system. It stays at the level of requirements: it states *what* the system relies on and *why*, not how it is designed or coded. Each decision is traced to the non-functional requirements (see `NON-FUNCTIONAL-REQUIREMENTS.md`), the concepts (see `CONCEPTS.md`) and the user stories (issues) it affects.

## Overview

The portal is hosted on AWS (see [nfr-hosting](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/NON-FUNCTIONAL-REQUIREMENTS.md#nfr-hosting)). It relies on GitHub for storage and versioning, and on Overleaf for writing articles:

- A **database repository** (see [c-database-repository](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-database-repository)) holds all results and consolidated data.
- Each replication has a **replication repository** (see [c-replication-repository](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-replication-repository)), hosted in the organizers' GitHub account or in an account chosen by the participant researcher.
- The **meter** (see [c-meter](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-meter)) turns the work of experimental subjects into standardized result files.
- Participants obtain their **draft article** (see [c-draft-article](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-draft-article)) as a project they can open in Overleaf.

## Decisions

### D1. GitHub is the storage and versioning layer
- **Status**: Accepted.
- **Context**: Results must be versioned, recoverable, editable with a comment, and publishable at the end of the project.
- **Decision**: All results and consolidated data are stored in a private GitHub repository of the organizers (the database repository). Every change is a commit; edit comments become commit messages. Versions are created automatically when a resource is added, edited or removed.
- **Consequences**: Restoration is a return to a previous commit. Publishing is done through a clean repository, not by changing the visibility of the original one (see D5).
- **Traceability**: [nfr-recovery](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/NON-FUNCTIONAL-REQUIREMENTS.md#nfr-recovery), [nfr-open-data](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/NON-FUNCTIONAL-REQUIREMENTS.md#nfr-open-data), [nfr-authorization](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/NON-FUNCTIONAL-REQUIREMENTS.md#nfr-authorization); [c-database-repository](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-database-repository); stories #12, #13, #14, #20.

### D2. AWS holds secrets and keys
- **Status**: Accepted.
- **Context**: The system acts on the organizers' GitHub account and keeps private data.
- **Decision**: The organizers' credentials are stored in AWS Secrets Manager. Data in the database repository are encrypted at rest with rotated keys; the system can decrypt them to read and modify the data.
- **Consequences**: Old versions must remain readable after key rotation, either by keeping old keys or by re-encrypting history. Decrypted data are not persisted outside the repository.
- **Traceability**: [nfr-secrets-management](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/NON-FUNCTIONAL-REQUIREMENTS.md#nfr-secrets-management), [nfr-encryption](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/NON-FUNCTIONAL-REQUIREMENTS.md#nfr-encryption), [nfr-hosting](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/NON-FUNCTIONAL-REQUIREMENTS.md#nfr-hosting), [nfr-recovery](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/NON-FUNCTIONAL-REQUIREMENTS.md#nfr-recovery); stories #9, #10, #13, #14.

### D3. Participants never give us their credentials
- **Status**: Accepted.
- **Context**: The organizers do not want to handle participants' API keys.
- **Decision**: Participants choose between two hosting modes. In the first, the replication repository is created in the organizers' account and the portal analyzes the work automatically. In the second, the participant downloads the repository as a ZIP and the meter, hosts the repository in their own account and uploads the standardized result files. The portal provides no automated support for the second mode beyond a good-practices guide.
- **Consequences**: Analysis through the portal and the closing of replication repositories (D5) are automated only in the first mode.
- **Traceability**: [nfr-confidentiality](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/NON-FUNCTIONAL-REQUIREMENTS.md#nfr-confidentiality); [c-replication-repository](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-replication-repository), [c-meter](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-meter); stories #9, #10, #11, #21, #22.

### D4. Draft articles are delivered to Overleaf
- **Status**: Proposed (the mechanism must be verified against the current Overleaf documentation).
- **Context**: Participants write their individual article in RTeX. The protocol is the same for everyone, the analysis derives from the meter, and the rest is written by the authors.
- **Decision**: The portal generates the draft article and offers it to the participant as a project that can be opened in Overleaf. No Overleaf credentials are requested from participants. The article receives only the per-replication consolidated data.
- **Consequences**: The article can contain project-status information in its introduction, but never data of other replications or global data. Git-based integration with Overleaf is discarded because it would require user tokens (see [nfr-confidentiality](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/NON-FUNCTIONAL-REQUIREMENTS.md#nfr-confidentiality)).
- **Traceability**: [nfr-data-isolation](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/NON-FUNCTIONAL-REQUIREMENTS.md#nfr-data-isolation), [nfr-confidentiality](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/NON-FUNCTIONAL-REQUIREMENTS.md#nfr-confidentiality); [c-draft-article](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-draft-article), [c-consolidated-data](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-consolidated-data); stories #16, #17.

### D5. Consolidation and clean repositories
- **Status**: Accepted.
- **Context**: Dashboards, draft articles and the final paper need derived data; closing and publishing must not leak identities or history.
- **Decision**:
  - *Per-replication consolidation* produces data of one replication only; *global consolidation* merges all replications. Both derive from the standardized result files and are regenerated when results are analyzed or edited. Consolidation is idempotent: the same inputs give the same output.
  - A *clean repository* (see [c-clean-repository](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-clean-repository)) is a new repository, without history or fork links, with anonymized content (including the work of experimental subjects). It is used to close a replication repository (the original becomes private) and to publish the database at the end of the project.
  - Experimental subjects may delete their forks if they wish; this does not affect the work already received. The notice is sent only after all submissions are merged or archived.
- **Consequences**: Visibility of replication repositories is changed by the participant researcher, because they may need them public for their own paper. The final repository is also created as a clean one.
- **Traceability**: [nfr-anonymization](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/NON-FUNCTIONAL-REQUIREMENTS.md#nfr-anonymization), [nfr-open-data](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/NON-FUNCTIONAL-REQUIREMENTS.md#nfr-open-data), [nfr-data-isolation](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/NON-FUNCTIONAL-REQUIREMENTS.md#nfr-data-isolation); [c-consolidated-data](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-consolidated-data), [c-clean-repository](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-clean-repository); stories #16, #17, #18, #20, #21.

## Open issues
- Mechanism and limits of the Overleaf delivery (D4).
- Key-rotation strategy that keeps previous versions readable (D2).
- Anonymization rules for the work of experimental subjects (D5).
