# Pull Requests Dataset

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19354721.svg)](https://doi.org/10.5281/zenodo.19354721)

This repository provides a comprehensive dataset of Pull Requests (PRs) collected from various GitHub repositories.

In addition to PR metadata, the dataset includes:
- Comments and reviews,
- Source code from both the base and head branches,
- Code metrics computed for each branch.

**Authors**: Mallory Bouchard, Hugo Raskin, Arthur Smoos, and Taj Eddine Temsamani Bouazza \
**Analysis dates**: From March to April 2026

## Repository Hierarchy

The dataset is organized according to the following directory structure:

```bash
analysis
└── organization/
    └── repository/
        └── pr_1/
            ├── head_{Oid}.zip
            └── base_{Oid}.zip
dump/
└── pull_requests
readme.md #You are here
```

Directory names strictly follow the organization name, repository name, and the target pull request number. 
Inside each PR directory, the source code of the head and base branches is stored in separate zip files, named according to their respective Git OIDs.

For example, the 9th pull request of the Spring Framework repository (organization: `spring-projects`, repository: `spring-framework`) is located at:

```
analysis/spring-projects/spring-framework/pr_9
```

## Metadata Dump

The `dump` directory contains a MongoDB dump comprising metadata associated with the dataset.
To access this data, the dump must be restored into a MongoDB instance using the following command:

1. Init a MongoDB instance
    ```bash
    docker run -d --name mongo -p 27017:27017 mongo
    ```
   
2. Copy the dump directory to the MongoDB container
    ```bash
    docker cp dump mongo:/dump
    ```
   
3. Restore the dump into the MongoDB instance
    ```bash
   docker exec -it mongo mongorestore /dump
   ```

4. Connect to the MongoDB instance and access the `pull_requests` collection to explore the data.

A detailed description of the stored data is provided in the next section.

## Data description

Each entry is a JSON object representing a unique PR analysis.

#### Primary Fields
| Field | Type | Description                                                             |
| :--- | :--- |:------------------------------------------------------------------------|
| `_id` | `String` | Unique identifier (e.g., `Org_Repo_backend_pr1`).                       |
| `analysed_at` | `Date` | Timestamp of the technical analysis (MongoDB `$date` format).           |
| `org` | `String` | GitHub Organization name.                                               |
| `repo` | `String` | Repository name.                                                        |
| `base` | `Object` | Code metrics of the target branch (before the PR).                      |
| `head` | `Object` | Code metrics of the source branch (after the PR).                       |
| `meta` | `Object` | GitHub metadata for the Pull Request.                                   |
 | `size` | `Object` | Relative size of Pull Request changes.                                  |
| `stats` | `Object` | Size and duration statistics.                                           |
| `comments` | `Array` | List of comments (human and bot-generated).                             |
| `reviews` | `Array` | List of code reviews performed.                                         |
 | `review_decision` | `String` | Overall review decision (`APPROVED`, `CHANGES_REQUESTED`, `COMMENTED`). |

---

### Object Details

#### 1. Code Metrics (`base` & `head`)
These objects measure the technical health of the source code. The metrics are computed using SonarCloud and include:
* **ncloc**: Non-Commenting Lines of Code.
* **complexity**: Cyclomatic complexity.
* **cognitive_complexity**: Cognitive complexity (readability).
* **duplicated_lines**: Number of duplicated lines of code.
* **code_smells**: Number of design issues detected.
* **development_cost**: Estimated development cost (based on code volume).
* **software_quality_maintainability_rating**: Maintainability rating (e.g., 1 = 'A').

#### 2. PR Metadata (`meta`)
* **id**: Internal GitHub identifier (e.g., `PR_kwDO...`).
* **number**: Pull Request number.
* **title**: Title of the PR.
* **body**: Description/Content of the PR.
* **state**: Current status (`MERGED`, `OPEN`, `CLOSED`).
* **created_at** / **closed_at** / **merged_at**: Lifecycle timestamps.
* **author**: Object containing `login` and `is_bot` (Boolean).

#### 3. Size Metrics (`size`)
* **additions**: Number of lines added in the PR.
* **deletions**: Number of lines removed in the PR.
* **changed_files**: Number of files modified in the PR.

#### 4. Statistics (`stats`)
* **head_size**: Relative size of the source branch.
* **base_size**: Relative size of the target branch.
* **total_time**: Total time elapsed (likely in minutes/hours).

---

### Interactions (`comments` & `reviews`)
The arrays contain objects detailing:
* **author**: Actor details (`login`, `is_bot`).
* **body**: Text content (Markdown, often including SonarCloud badges).
* **state** (Reviews only): Validation status (`COMMENTED`, `APPROVED`, `CHANGES_REQUESTED`).
* **created_at** / **submitted_at**: Timestamp of the interaction.
