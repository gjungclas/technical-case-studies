# Technical Case Studies & System Patches

A curated collection of real-world debugging logs, architectural problem-solving, root-cause analyses, and upstream open-source contributions. 

This repository documents hands-on solutions to broken toolchains, defensive input handling, schema adaptations, and systems integration challenges encountered across academic research, software engineering projects, and open-source ecosystems.

---

## Repository Index

| Project / Case Study | Domain / Stack | Key Techniques & Focus Areas | Upstream / Reference |
| :--- | :--- | :--- | :--- |
| [`word-apa7-engine-patch`](./word-apa7-engine-patch/) | XML, XSLT 1.0, Office Open XML | Defensive URI normalization, string evaluation, legacy engine modernization | https://github.com/gjungclas/APA7-references-word |

---

## Case Study Summaries

### [1. Microsoft Word APA 7th XSL Citation Engine Patch](./word-apa7-engine-patch/)
* **Problem:** Microsoft Word (Microsoft 365 enterprise installations) lacks native engine-level support for APA 7th edition formatting. Deploying community-built XSLT stylesheets introduced regressions: duplicate protocol prefixes (`https://doi.org/https://doi.org/...`) and improper colon delimiters (`: `) preceding web URLs.
* **Root Cause:** Word’s internal bibliography parser treats DOIs as untyped string literals rather than normalized URIs and relies on a global enumeration separator rather than sentence-terminating punctuation in `templateRDAFU`.
* **Solution:** Reverse-engineered the local `.XSL` stylesheet in `%appdata%\Microsoft\Bibliography\Style`. Implemented conditional protocol evaluation (`starts-with()`) to normalize both raw DOIs and active URIs idempotently, while updating web template delimiters to strict APA 7th specifications.
* **Artifacts:** Patched `.XSL` stylesheet, technical root-cause breakdown, and open-source Pull Request submission.

---

## Engineering Methodology

Every case study in this repository adheres to a disciplined engineering workflow:

1. **Failure Isolation:** Characterizing unexpected behaviors, formatting failures, or edge-case breakdowns under strict evaluation or production constraints.
2. **Root-Cause Analysis (RCA):** Inspecting raw schemas, tracing internal execution lifecycles, and distinguishing tooling defects from user input discrepancies.
3. **Defensive Implementation:** Engineering robust, backwards-compatible fixes that avoid fragile manual overrides and handle variable inputs gracefully.
4. **Verification & Upstream Contribution:** Validating results across production environments, documenting regression test scenarios, and contributing patches back to upstream open-source maintainers where applicable.

---

## Author

**Grace Jungclas**  
*Computer Science undergraduate at Southern New Hampshire University*  
* [GitHub Profile](https://github.com/)
