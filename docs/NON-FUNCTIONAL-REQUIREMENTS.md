# Non-functional requirements

- <a id="nfr-authentication"></a>[nfr-authentication] The system shall require researchers (see [c-researcher](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-researcher)) to register using an external identity provider, such as Google. After registration, the interested researchers (see [c-interested-researcher](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-interested-researcher)) shall receive a confirmation email and a drop-out link.

- <a id="nfr-participation"></a>[nfr-participation] Request for participation shall be validated by an organizer (see [c-organizer](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-organizer)).

- <a id="nfr-logging"></a>[nfr-logging] The access to replication materials (see [c-replication-material](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-replication-material)) and results (see [c-replication-result](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-replication-result)) shall be logged when it takes place through the portal. Replication repositories are public, so direct access to them is not logged.

- <a id="nfr-hosting"></a>[nfr-hosting] The system shall be hosted entirely on AWS.

- <a id="nfr-confidentiality"></a>[nfr-confidentiality] The system shall not request, receive or store credentials or API keys of participant researchers (see [c-participant-researcher](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-participant-researcher)).

- <a id="nfr-secrets-management"></a>[nfr-secrets-management] Credentials of the organizers (see [c-organizer](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-organizer)), such as the GitHub access token, shall be stored in AWS Secrets Manager.

- <a id="nfr-authorization"></a>[nfr-authorization] Only the participant researchers of a replication (see [c-participant-researcher](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-participant-researcher)) can modify its results (see [c-replication-result](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-replication-result)).

- <a id="nfr-recovery"></a>[nfr-recovery] Replication results shall be stored in a version-controlled repository (see [c-database-repository](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-database-repository)), so that any previous version can be restored. Versions shall be created automatically when a resource is added, edited or removed.

- <a id="nfr-encryption"></a>[nfr-encryption] While the database repository (see [c-database-repository](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-database-repository)) is private, its data shall be encrypted at rest with periodic key rotation, and shall remain readable and modifiable by the system. Previous versions shall remain recoverable after key rotation.

- <a id="nfr-anonymization"></a>[nfr-anonymization] Data and work of experimental subjects (see [c-experimental-subject](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-experimental-subject)) shall be anonymized before being included in a clean repository (see [c-clean-repository](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-clean-repository)).

- <a id="nfr-open-data"></a>[nfr-open-data] The database repository (see [c-database-repository](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-database-repository)) shall be private during the project and shall be publishable at its end through a clean repository (see [c-clean-repository](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-clean-repository)).

- <a id="nfr-data-isolation"></a>[nfr-data-isolation] A draft article (see [c-draft-article](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-draft-article)) shall have access only to the per-replication consolidated data of its own replication (see [c-consolidated-data](https://github.com/GRISE-UPM/testing-bias-replication-centrer/blob/main/docs/CONCEPTS.md#c-consolidated-data)), so that changing filters or queries cannot reveal data of other replications.
