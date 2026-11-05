# Product Requirements Document: Doc Management

**Status:** Draft for Product Owner review  
**Version:** 0.1  
**Product:** AI-assisted family document archive and action assistant

## Problem

Family documents are scattered across paper folders and digital files. Finding the right invoice, warranty proof or tax-relevant school payment is slow and occasionally impossible. School letters may announce an event months in advance, and preparations or deadlines are forgotten.

## Users

- Two adult household members, initially.
- Family members may benefit from calendar entries, but direct access for children is **out of scope** until permissions and privacy are designed.

## Outcomes (ordered by priority)

1. **Store:** Preserve an original digital copy of each document for later consultation.
2. **Find and understand:** Search by name, type, full text and metadata, and find the correct supporting document for a concrete question.
3. **Act and analyse:** Detect dates, tasks and payment information; propose calendar actions and support accurate household cost summaries.

## Example user journeys

- **Warranty:** Upload a computer receipt and later retrieve the purchase proof. The application may report a warranty end date **only when a relevant warranty term is known**, and must distinguish voluntary warranties from statutory consumer rights.
- **Tax:** Find school payment documents for a particular tax year, inspect originals and extract relevant amounts without assuming every document is tax-deductible.
- **School trip:** Upload a letter announcing a future event; detect event time, deadline, location, necessary items and any payment; propose a calendar entry and preparation tasks for review.
- **Household costs:** Retrieve water invoices and settlements for a specified period; distinguish prepayments, final bills and corrections to avoid double counting.

## First release (M1): Trusted document archive

- Upload and persist a PDF; store metadata separately from original file bytes.
- List, search by filename and open/download the original.
- Display processing state and actionable errors.
- Verify the files survive an application restart.
- Use fictional or redacted documents in development and public demonstration environments.

**M1 is useful without any LLM.** OCR, semantic search and automation follow in subsequent milestones.

## Next milestones

- **M2: Search and understanding.** Extract text from text-based/scanned PDFs, classify documents, full-text and metadata search, then RAG Q&A with source/page references and uncertainty handling.
- **M3: Action proposals.** Extract deadlines/events into structured objects. Before creating any Google Calendar event or task, require explicit user approval, show the source evidence, and prevent duplicate creation.
- **M4: Cost insights.** Extract amounts and billing periods, review document relationships, support category corrections and track household expenses without double counting.
- **M5: Hardening and public demo.** Authentication, encrypted transport, backup/restore tests, monitoring, quotas, and a separate demo with fictional documents only.

## Safety, privacy and quality requirements

- Never commit real household documents, personal data, secrets or OAuth tokens to the public repository.
- Treat uploaded documents as untrusted input, including text that looks like instructions to an AI.
- Separate real family data from the public demo and test fixtures.
- Treat OCR/extracted values and classifications as fallible; preserve original sources.
- No automatic calendar/task modifications without user approval in the initial product.
- Protect document access through authenticated users and explicit authorisation before using real family data.
- Keep extraction evidence and original document references for review.

## Proposed product success measures (targets subject to PO approval)

- A family member can retrieve an uploaded document without consulting physical files.
- Test with at least 20 **fictional** sample documents that include ambiguous and duplicate cases.
- Evaluate filename/text/type retrieval separately from RAG question answering.
- Track successful task extraction, missed deadlines, incorrect dates and false calendar proposals.
- Measure the time to find relevant documents against a manual baseline before setting a reduction target.

## Open product decisions

1. Should classification and extraction run automatically after upload, or only on request? **Proposed:** automatic after upload, with no external write actions until approved.
2. Is household access initially shared between both adults, or should each document have a separate owner/visibility policy?
3. For the first release, should the application store original documents in a managed filesystem volume or use an existing document archive integration?
4. Which document types should be the first ten sample cases (school letters, utilities, receipts, etc.)?

## Implementation context (not product requirements)

- Repository folders: `fe/` (Angular), `be/` (Python/FastAPI), `infra/` (Docker/PostgreSQL), `docs/`.
- Initial hosting: local development; private Raspberry Pi deployment subject to resource tests; separate cloud demo.
- Avoid adding agent frameworks, vector stores or OCR engines until the specific milestone needs them.
