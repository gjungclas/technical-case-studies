# Microsoft Word APA 7th Engine Patch

## Context & Motivation

While completing technical research coursework in data technologies, strict evaluation rubrics required exact adherence to APA 7th edition formatting. Feedback on an earlier submission specifically targeted automated bibliography artifacts:

> *"You included supporting citations and references, but several entries have APA formatting issues, including outdated retrieval formatting and an extra ampersand in one author listing."*

In rigorous academic and production environments, compliance standards frequently act as a hard filter. When automated tooling produces discrepancies—such as legacy retrieval phrasing or malformed author delimiters—it demonstrates how even minor toolchain quirks can compromise evaluation criteria. Rather than manually stripping these recurring artifacts in static text prior to every submission, I treated the toolchain breakdown as an engineering defect to resolve at the source.

Despite running the latest enterprise installation of Microsoft Word via Microsoft 365 (Version 2609 Build 16.0.20430.20032 64-bit), Word’s native citation engine remains hardcoded around legacy APA 6th paradigms. When executing dynamic bibliographies via community APA 7th stylesheets, the engine introduced automated regressions: prepending duplicate DOI URL schemes and appending outdated enumeration colons before web links. To eliminate these regressions permanently, I reverse-engineered Word's Extensible Stylesheet Language (`.XSL`) transformation template to implement defensive URI handling and enforce true APA 7th compliance at the engine level.

## Summary of Changes

### 1. Dynamic DOI Prefix Normalization
* **Issue:** Setting `doiPrefix` statically to `https://doi.org/` caused duplicated prefixes (`https://doi.org/https://doi.org/...`) when sources contained full URLs. Omitting the prefix left raw DOIs (`10.xxxx/...`) unlinked.
* **Fix:** Implemented an XSL conditional check using `starts-with($doi, 'http://') or starts-with($doi, 'https://')` to dynamically detect existing protocols, preserving fully qualified URIs while automatically prepending `https://doi.org/` to raw DOIs.

### 2. Web Source Delimiter Correction
* **Issue:** In `templateRDAFU`, the separator between `InternetSiteTitle` and `URL` referenced `templ_prop_EnumSeparator`, which emitted a colon (`: `) before the URL (`Website Name: https://...`).
* **Fix:** Replaced the localized enum call with an explicit period and space (`. `) to align with standard APA 7th syntax (`Website Name. https://...`).

## Verification & Impact
* Validated locally on Microsoft Word (Build 16.0+) by deploying the patched `.XSL` directly to `%appdata%\Microsoft\Bibliography\Style`.
* Tested across multiple source classes including academic journals, conference proceedings, preprints (arXiv, SSRN, ResearchGate), and technical documentation.
* Verified that dynamic bibliography refreshes consistently produce clean, submission-ready APA 7th citations without requiring post-generation manual overrides.
