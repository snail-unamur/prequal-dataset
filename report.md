# Pull Request Datasets

This repository is a comprehensive dataset of Pull Request (PR) from various Java repository.
In addition of metadata of the PR, the datasets includes information on comments, reviews, code on the base and head branch, and also code metrics on both branches.

**Authors:** Mallory Bouchard, Hugo Raskin, Arthur Smoos, and Taj Eddine Temsamani Bouazza \
**Year:** April 2026

___
## Repository Hierarchy

The hierarchy of the dataset is structured as followed:

```bash
analysis
└── organization/
    └── repository/
        └── pr_1/
            ├── head.zip
            └── base.zip
dump/
└── pull_requests
readme.md (You are here)
```

The naming convention of the directories strictly follows the organization, repository name, and the number of the target PR.
For example, the 9th PR of Spring Framework (organization: `spring-projects` and repo: `spring-framework`), can be found at `analysis/spring-projects/spring-framework/pr_9`.

The `dump` directory is a Mongo dump of a database comprehsing metadata on the dataset. 
It must be imported in a Mongo instance to retrieve the data:

```bash
mongorestore -h host.com:port -d your_db_instance -u username -p password dump/pull_requests
```

The description of the data is described in the next section.

## Data description

- Describe prequal
- Describe dataset project
- Describe mongo scheme
