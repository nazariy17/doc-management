# Initial Product Backlog

**Status:** Draft for PO review. Ordering reflects user pain and system dependencies, not fixed dates.

| ID | Priority | Story / capability | Acceptance evidence |
| --- | --- | --- | --- |
| DOC-001 | P0 | As a household member, I can upload a PDF and preserve its original. | Upload, persistence after restart, original can be retrieved; invalid file is rejected. |
| DOC-002 | P0 | As a household member, I can list, open and download stored documents. | List shows filename, upload date, status; original opens and downloads correctly. |
| DOC-003 | P0 | As a household member, I can search by filename and available document metadata. | Searches return correct records and useful empty states. |
| DOC-004 | P1 | As a household member, I can search the contents of PDFs and scanned letters. | Text extraction/OCR is tracked per page, searchable; failures are visible. |
| DOC-005 | P1 | As a household member, I can distinguish invoices, receipts, letters and other documents. | Classification has a source, confidence and manual correction path. |
| DOC-006 | P1 | As a household member, I can ask a question and get an evidence-linked answer. | RAG response cites document/page; refuses unsupported answers. |
| DOC-007 | P1 | As a household member, I can review future appointments, deadlines and needed items. | Proposed date and action show source evidence and can be corrected. |
| DOC-008 | P1 | As a household member, I can approve a Google Calendar action. | Preview, explicit consent, idempotent creation and clear success/failure state. |
| DOC-009 | P2 | As a household member, I can review household expenses by category/period. | Avoids counting prepayments and settlement totals twice; allows correction. |
| DOC-010 | P2 | As a household member, I can find purchase evidence and warranty information. | Shows original receipt; warranty end date only if terms are evidenced. |

## First development slice: DOC-001 + DOC-002

**Scenario A: successful upload**
- Given a valid text-based PDF of permitted size, when I upload it, then a unique document record and original file are stored, and the UI shows it in the list.

**Scenario B: restart**
- Given an uploaded document, when the backend and database containers restart, then metadata and the original file remain accessible.

**Scenario C: invalid upload**
- Given a non-PDF renamed to `.pdf`, an oversized file or an unreadable document, when I upload it, then the API rejects it with a meaningful error without storing an orphaned record.

**Scenario D: retrieve**
- Given an uploaded document, when I select it from the list, then I can open/download the exact original.

**Engineering acceptance**
- Automated API tests for success, invalid file and restart/retrieval path.
- Document storage is not inside the Git repository; local persistent Docker volumes or mounted directories are configured.
- File size limits, UUID identifiers, safe filenames and content validation are defined.
- README documents development startup and how to run tests.
- The PR explains one significant design choice (file bytes in filesystem vs database) and its trade-off.

## Backlog management conventions

- P0 means necessary to make the archive useful.
- P1 adds retrieval intelligence and approved workflow actions.
- P2 addresses specialised analysis and warranty scenarios.
- Additional scope must be justified by a user problem, not by the desire to try a framework.
