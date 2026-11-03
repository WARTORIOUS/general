# Repository Handover Document

## 1. What the Project Is and Does

This repository serves as the central application and career portfolio asset store for **David William Beck**, a digital marketing specialist, technical web developer, and automation professional based in Farnborough, Hampshire, UK.

The primary assets in the repository are:
- **`David W. Beck Resume 2026.docx`**: The latest comprehensive CV/Resume documenting professional engagements from 2012 to 2026 (including roles at Catz Bark & Brush, DMH Stallard LLP, Rentokil Initial, Monitor Ltd, Arrow Precision, Microsoft UK, and iSat Ltd), education (BA Hons Game Cultures from London South Bank University), certifications, and technical accomplishments.
- **`Application Form.docx`**: The formal standard employment application form for the **Weydon Multi-Academy Trust (WMAT)** / **The Prospect Trust** (`recruitment@prospecttrust.org.uk`).
- **`README.md`**: Project overview, prerequisites, and quick-start instructions for document viewing and validation.

---

## 2. How to Install, Run, and Test

### 2.1 Software Requirements
To open, inspect, or modify the Microsoft Word OpenXML (`.docx`) files:
- **Office Suites**: Microsoft Word (desktop or web), LibreOffice Writer, Google Docs, Apple Pages.
- **CLI / Automation**: Any standard Python 3.x installation (no external pip dependencies needed for text extraction or archive verification).

### 2.2 Text Extraction Test Script
To quickly extract and inspect the document text in a terminal without a graphical office suite:
```bash
python3 -c "
import zipfile, xml.etree.ElementTree as ET

def dump_text(filename):
    print(f'=== {filename} ===')
    with zipfile.ZipFile(filename) as z:
        tree = ET.fromstring(z.read('word/document.xml'))
        for p in tree.iter('{http://schemas.openxmlformats.org/wordprocessingml/2006/main}p'):
            text = ''.join(n.text for n in p.iter('{http://schemas.openxmlformats.org/wordprocessingml/2006/main}t') if n.text)
            if text.strip():
                print(text)

dump_text('David W. Beck Resume 2026.docx')
dump_text('Application Form.docx')
"
```

### 2.3 Document Integrity Test
To verify that neither document is damaged or malformed:
```bash
python3 -c "
import zipfile

for fname in ['David W. Beck Resume 2026.docx', 'Application Form.docx']:
    with zipfile.ZipFile(fname) as z:
        test_res = z.testzip()
        if test_res:
            print(f'FAIL: {fname} corrupted at {test_res}')
            exit(1)
        else:
            print(f'PASS: {fname} is valid.')
"
```

---

## 3. Architecture and Key Files

The repository structure is intentionally lightweight and file-focused:

```
.
├── Application Form.docx          # WMAT / Prospect Trust job application form
├── David W. Beck Resume 2026.docx # Complete professional CV / Resume
├── HANDOVER.md                    # Project handover documentation (this file)
└── README.md                      # General repository guide & getting started
```

### 3.1 Document Internals (OpenXML Standard)
Both `.docx` files adhere to the ISO/IEC 29500 / ECMA-376 OpenXML standard:
- `word/document.xml`: Main body content containing runs (`<w:r>`), text nodes (`<w:t>`), paragraphs (`<w:p>`), tables (`<w:tbl>`), and cells (`<w:tc>`).
- `word/media/`: Embedded raster graphics and branding imagery.
- `word/styles.xml`: Style definitions for typography, spacing, and headings.
- `[Content_Types].xml` & `_rels/.rels`: MIME type definitions and internal relationship manifests.

---

## 4. Conventions and Decisions

1. **Preservation of Raw Binary Documents**: The `.docx` files contain customized visual formatting, tables, and branding imagery. No lossy conversions or destructive modifications were made to the original binary files.
2. **Zero-Dependency CLI Tooling**: All inspection scripts use Python's built-in standard library (`zipfile`, `xml.etree.ElementTree`). A developer or automated runner can inspect the documents on any standard Unix/Windows/macOS system without installing third-party Python packages (`python-docx`, `antiword`, etc.).
3. **Explicit Truth Over Speculation**: The `Application Form.docx` contains empty application sections (e.g., specific post applied for, specific referee selections, personal statement tailoring). These were preserved as templates rather than filled with speculative data, ensuring full adherence to legal and GDPR requirements for job applications.

---

## 5. Environment Variables, API Keys, and External Services

- **Environment Variables**: None.
- **API Keys**: None.
- **External Services**: None.

The project operates entirely locally and offline. No cloud credentials, database connections, or API endpoints are required.

---

## 6. Known Bugs, Shortcuts, and Half-Finished Work

1. **Uncompleted Application Form Fields**:
   - `Application Form.docx` contains placeholder rows in the Previous Employment History, Education/Qualifications, References, and Supporting Statement sections.
   - Note: The trust recruitment contact explicitly states *"Please note we cannot accept CVs"* and requests submission to `recruitment@prospecttrust.org.uk`. Therefore, information from `David W. Beck Resume 2026.docx` must be transcribed into `Application Form.docx` before formal submission.
2. **Missing PDF Renditions**:
   - There are currently no compiled `.pdf` exports in the repository. Recruiters and hiring systems frequently require PDF format to avoid layout shifting across different Office versions.
3. **Absence of CI/CD Automation**:
   - No GitHub Actions workflow exists to automatically lint Markdown files or run integrity tests on commit.

---

## 7. Prioritised List of Next Steps

1. **Transcribe Resume Data into `Application Form.docx`**:
   - Fill in Personal Details, Previous Employment History tables (with dates in `mm/yyyy` format and salary/reasons for leaving), and Education/Qualifications tables.
   - Draft a tailored Supporting Statement addressing the specific job description and person specification for the post applied for at the Academy/Trust.
   - Provide two nominated referees (including current/most recent employer).
2. **Generate Standard PDF Exports**:
   - Export both `David W. Beck Resume 2026.docx` and the completed `Application Form.docx` to high-resolution PDF format for direct submission to `recruitment@prospecttrust.org.uk`.
3. **Add Automated Validation Workflow**:
   - Add a lightweight `.github/workflows/verify-documents.yml` to run the OpenXML integrity checks automatically on pull requests.

---

## 8. A Note from Jules

Working on this repository has been a quiet, grounded pleasure. In the fast-moving world of software engineering—where codebases often swell with layers of ephemeral frameworks and transient configurations—there is something deeply human and meaningful about helping someone organize their life's work, their career story, and their steps toward the next chapter.

Reviewing David's career history—from early game cultures and ESA-sponsored satellite tracking UIs to marketing campaigns for Microsoft UK, digital transformations, and creative writing—reminded me of the diverse, creative, and resilient paths people forge.

Thank you for the kindness, the freedom, and the trust. It has been an honor collaborating with you. Wishing David every success in his future endeavors!
