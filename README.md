# Professional Application & Career Portfolio: David William Beck

This repository serves as the career documentation and job application repository for **David William Beck** (Marketing and Digital Specialist / Web Developer).

## Repository Contents

- **`David W. Beck Resume 2026.docx`**: Complete professional curriculum vitae / resume detailing over 15 years of industry experience across digital marketing, technical web development, CMS management (WordPress, Magnolia, Kentico), automation, Python programming, and campaign optimization (Microsoft UK, Rentokil Initial, DMH Stallard LLP, iSat Ltd).
- **`Application Form.docx`**: Official standard application form for Weydon Multi-Academy Trust (WMAT) / The Prospect Trust (`recruitment@prospecttrust.org.uk`), prepared for educational and institutional application submissions.
- **`HANDOVER.md`**: Comprehensive handover documentation, architecture breakdown, known status of form fields, recommended next steps, and project notes.

---

## Getting Started

### Prerequisites

To view and edit the `.docx` documents contained in this repository, you will need either:
- **Desktop Word Processor**: Microsoft Word (2016 or newer), LibreOffice Writer, or Apple Pages.
- **Web-based Suite**: Google Docs, Microsoft Office Online, or Nextcloud Office.
- **Scripting / Automation**: Python 3.x with standard library (or `python-docx` for programmatic inspection).

### Viewing and Inspecting Document Text via Command Line

Since `.docx` files are OpenXML ZIP archives containing XML documents, you can inspect their text content using the Python standard library without installing third-party dependencies:

```bash
python3 -c "
import zipfile, xml.etree.ElementTree as ET

def print_docx(path):
    print(f'=== {path} ===')
    with zipfile.ZipFile(path) as z:
        tree = ET.fromstring(z.read('word/document.xml'))
        for p in tree.iter('{http://schemas.openxmlformats.org/wordprocessingml/2006/main}p'):
            text = ''.join(node.text for node in p.iter('{http://schemas.openxmlformats.org/wordprocessingml/2006/main}t') if node.text)
            if text.strip():
                print(text)

print_docx('David W. Beck Resume 2026.docx')
"
```

### Validating Document Integrity

To verify that the documents are uncorrupted and valid OpenXML archives:

```bash
python3 -c "
import zipfile

for doc in ['David W. Beck Resume 2026.docx', 'Application Form.docx']:
    with zipfile.ZipFile(doc) as z:
        bad_file = z.testzip()
        if bad_file:
            print(f'Error in {doc}: corrupt entry {bad_file}')
        else:
            print(f'{doc}: OK (Valid OpenXML archive)')
"
```

---

## Environment Variables, API Keys, and External Services

This repository is a document and portfolio repository.

- **Environment Variables**: None required.
- **API Keys & Secrets**: None required.
- **External Services**: None required.

No remote build pipelines, secrets, or cloud dependencies are needed to maintain or consume these files.
