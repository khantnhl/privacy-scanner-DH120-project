# What Are You About to Upload? — local privacy scanner (demo)

**Demonstration scanner: does not detect all sensitive information.**

A single-page tool that runs in the browser. It flags common identifiers in a .txt/.md/.pdf file,
redacts them with placeholders, shows the original and cleaned versions side by side, and lets you
download the cleaned text before you paste it into an AI service.

## Run

```bash
cd privacy-scanner && python3 -m http.server 8000
```

Open http://localhost:8000. A local server is needed only because browsers block the PDF worker on `file://`.

## Demo script

1. Open DevTools → **Network**, then press **Clear**.
2. Choose `sample-student-note.pdf` (or `.txt`, or click **Load fictional sample**).
3. Click **Scan**: the name, student ID, email, phone and mock API key are highlighted.
4. Click **Redact**: the cleaned version appears next to the original.
5. Point out that *"the only first-year transfer leading the robotics club"* is still there. That's an
   indirect identifier, and pattern matching can't catch it.
6. Click **Download cleaned .txt**.
7. Show that the Network panel recorded no request during the scan. The page also sets
   `connect-src 'none'`, so it couldn't send a request even if it tried. This shows how *this scanner* behaved
   during the test. It says nothing about how any external chatbot handles data.

## What it detects (regex)

Labelled names (`Name: …`), student IDs (`Student ID: …` or `###-###-###`), emails, US phone numbers,
SSNs, IPv4 addresses, and common API key formats (`sk-…`, `AKIA…`, `ghp_…`, `AIza…`, `xox…-`).

All data in the sample is fictional. pdf.js (Apache-2.0) is in `vendor/`, so nothing is loaded from a CDN.
