# Pull Request Datasets

This repository provides a comprehensive dataset of Pull Requests (PRs) collected from various Java repositories.

In addition to PR metadata, the dataset includes:
- Comments and reviews,
- Source code from both the base and head branches,
- Code metrics computed for each branch.

**Authors**: Mallory Bouchard, Hugo Raskin, Arthur Smoos, and Taj Eddine Temsamani Bouazza \
**Year**: April 2026 \
**Analysis dates**: From March to April 2026

## Repository Hierarchy

The dataset is organized according to the following directory structure:

```bash
analysis
└── organization/
    └── repository/
        └── pr_1/
            ├── head.zip
            └── base.zip
dump/
└── pull_requests
readme.md #You are here
```

Directory names strictly follow the organization name, repository name, and the target pull request number.

For example, the 9th pull request of the Spring Framework repository (organization: spring-projects, repository: spring-framework) is located at:

```
analysis/spring-projects/spring-framework/pr_9
```

## Metadata Dump

The dump directory contains a MongoDB dump comprising metadata associated with the dataset.
To access this data, the dump must be restored into a MongoDB instance using the following command:

```bash
mongorestore -h host.com:port -d your_db_instance -u username -p password dump/pull_requests
```

A detailed description of the stored data is provided in the next section.

## Data description

- Describe prequal
- Describe dataset project
- Describe mongo scheme
