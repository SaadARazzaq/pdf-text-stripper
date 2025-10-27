# 🧾 PDFTextStripper

**PDFTextStripper** is a lightweight Python utility that removes *only the text layer* from PDF files — while preserving **images, colors, vector drawings, and overall structure**.  

Unlike most PDF "cleaning" tools that rasterize or flatten the document (destroying quality), this script intelligently redacts and deletes the text stream using **PyMuPDF (fitz)** redaction annotations, ensuring that the visual content remains completely intact.

---

## Features

- ✅ Removes **all text** while keeping **graphics, shapes, and images** untouched  
- ✅ Preserves **original PDF structure and layers**  
- ✅ Fast and memory-efficient (stream-based processing)  
- ✅ Simple to use and modify  
- ✅ No external dependencies except PyMuPDF

---

## How It Works (Detailed Logic)

This script uses **PyMuPDF**’s low-level PDF parsing and redaction API to selectively remove only text blocks from each page.

### Step-by-Step Logic:

1. **Open the PDF:**
   ```python
   doc = fitz.open(input_pdf)

The file is loaded into a `fitz.Document` object, allowing direct access to all pages and content streams.

2. **Iterate through pages:**

   ```python
   for page_num in range(len(doc)):
       page = doc[page_num]
   ```

   Each page is processed individually.

3. **Extract text structure:**

   ```python
   blocks = page.get_text("rawdict")
   ```

   The `"rawdict"` mode provides a dictionary of all content blocks (text, images, paths, etc.), with exact coordinates (`bbox`) and type information.

4. **Detect and mark text blocks:**

   ```python
   if b["type"] == 0:  # text block
       page.add_redact_annot(b["bbox"])
   ```

   Each text block is tagged for redaction using its bounding box.
   This does **not yet remove text**, it just *marks it*.

5. **Apply redactions:**

   ```python
   page.apply_redactions(images=fitz.PDF_REDACT_IMAGE_NONE)
   ```

   This command finalizes the redaction process:

   * All marked text is permanently deleted.
   * Images, vectors, and colors remain untouched.
   * The visual layout of the page is fully preserved.

6. **Save the cleaned PDF:**

   ```python
   doc.save(output_pdf, deflate=True, garbage=4)
   ```

   The result is written to disk efficiently, with optional compression and garbage collection to minimize file size.

---

## Results

Below is a visual comparison showing how **PDFTextStripper** removes all text while preserving images, graphics, and color layers perfectly.

| 📝 **Input PDF (With Text)** | 🧼 **Output PDF (Text Removed)** |
|------------------------------|----------------------------------|
| <img width="294" height="419" alt="Input PDF" src="https://github.com/user-attachments/assets/8c51f423-b7b6-4095-9716-9d93420ab497" /> | <img width="292" height="420" alt="Output PDF" src="https://github.com/user-attachments/assets/bdc4ee11-8a71-4724-8be1-4f2389f60c3e" /> |

✨ Notice how all text elements are removed, yet **graphics, colors, and vector elements** remain completely intact — preserving the original design quality.

---

## Why This Works Better

Most PDF cleaning tools either:

* **Rasterize** the page (convert to image → loses quality), or
* **Flatten** content (merging layers → loses editability), or
* **Strip streams** blindly (removes shapes and images too).

`PDFTextStripper` uniquely leverages **PyMuPDF’s redaction API**, which is natively supported by the PDF spec — making this a *non-destructive*, *standards-compliant*, and *high-fidelity* approach.

---

## Requirements

Install dependencies with:

```bash
pip install -r requirements.txt
```

---

## Usage

1. Place your `input.pdf` in the project directory.
2. Run:

   ```bash
   python remove_text.py
   ```
3. The cleaned PDF will be saved as `output_cleaned.pdf`.

---

## Project Structure

```
pdf-text-stripper/
│
├── remove_text.py          # Core logic
├── requirements.txt        # Dependencies
├── README.md               # Documentation
└── input.pdf               # (Your test file)
```

---

## Author

**Saad Abdur Razzaq**

AI & ML Engineer | Effixly

📬 [Follow me on LinkedIn](https://linkedin.com/in/saadarazzaq)

📧 [Mail Me: sabdurrazzaq124@gmail.com](mailto:sabdurrazzaq124@gmail.com)

---

## Acknowledgements

* [PyMuPDF Documentation](https://pymupdf.readthedocs.io/en/latest/)
* Community discussions on preserving PDF layers inspired this approach

---

> ⚡ *PDFTextStripper — the only open-source Python tool that removes text while keeping your PDF’s visuals perfectly intact.*

---

Would you like me to also give you a **short Git command sequence** (for initializing and pushing this repo to GitHub with your first commit)?
