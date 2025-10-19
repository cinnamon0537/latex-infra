# LaTeX Protocol Template

Automated LaTeX workflow - write in `.tex`, get a PDF URL automatically.

## 🚀 What This Does

1. You edit `src/protocol.tex`
2. GitHub automatically builds it to PDF
3. PDF is available at a permanent URL

## 📄 Access PDF

**[View Latest PDF](./protocol.pdf)**

After forking, this same link works in your repo.

## 📁 Files

- `src/protocol.tex` - Your content (edit this)
- `.github/workflows/build-pdf.yml` - Auto-build config
- `.gitignore` - Prevents PDF conflicts
- `protocol.pdf` - **Auto-deployed** (not in git)

## 🛠️ Setup

1. **Fork this repo**
2. **Enable Pages**: Settings → Pages → GitHub Actions → Save
3. **Edit** `src/protocol.tex` 
4. **Commit & push** - PDF builds automatically in ~2 min
5. **Share**: `https://YOURNAME.github.io/REPO/protocol.pdf`

## 🔧 Local Test

```bash
cd src
pdflatex protocol.tex  # Generates PDF locally