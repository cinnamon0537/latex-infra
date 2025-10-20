# LaTeX Protocol Automation

Automated LaTeX workflow — write in `.tex`, get a compiled `.pdf` in the same folder automatically.

## 🚀 What This Does

1. You create or edit `.tex` files anywhere under `src/`, for example:
   ```
   src/hcd/exercise-5/protocol.tex
   ```
2. GitHub Actions detects changes to `.tex` files on every push to `main`.
3. Only changed `.tex` files are recompiled to `.pdf` using LaTeX.
4. The resulting `.pdf` is placed **in the same directory** as the source `.tex`.
5. All build artifacts (including PDFs) are ignored in git — they are never committed manually.

---

## 📄 Access PDF

Each `.tex` file produces a `.pdf` with the same name inside its own directory, e.g.:

```
src/hcd/exercise-5/
 ├── protocol.tex
 └── protocol.pdf
```

---

## 📁 Files & Structure

```
src/
 ├── hcd/
 │    └── exercise-5/
 │         ├── protocol.tex
 │         ├── images/
 │         │    └── example.png
 │         └── protocol.pdf (generated automatically)
 ├── another-course/
 │    └── exercise-1/protocol.tex
```

- `.github/workflows/build-pdfs.yml` — CI workflow for auto-building PDFs  
- `.gitignore` — ignores all `.pdf`, `.log`, and other LaTeX artifacts  
- `src/**/` — contains course folders, exercises, and their `.tex` sources  

---

## 🔧 Local Test

To compile a single exercise locally:

```bash
cd src/hcd/exercise-5
pdflatex protocol.tex
```

To compile everything recursively (all `.tex` files under `src/`):

```bash
find src -name "*.tex" -exec pdflatex {} \;
```

---

## 💡 Notes

- PDFs are **never committed manually** — the CI workflow handles all builds.  
- The setup scales naturally across multiple courses and semesters.
