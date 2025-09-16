# 🔐 Secure Wipe Certificate – Offline Generator & Verifier

This project lets you:

1. **Generate** digitally signed Secure Wipe Certificates as PDFs with embedded verification payloads and QR codes.
2. **Verify** those certificates completely offline via CLI.

> **Note:** No browser verifier code is included here — this repo is for local/offline use only.

---

## 📂 Project Structure

```
.
├── main.py               # Entry point for certificate generation
├── pdf_gen.py            # Builds PDF and embeds /CertPayload
├── verifier.py           # Offline CLI verifier
├── sign.py               # Signing & verification helpers
├── payload_utils.py      # Canonical JSON helpers
├── qr_utils.py           # QR code generator helper
├── config.py             # Config (QR size, key paths)
├── keys/                 # Your keys (NOT committed)
├── .env                  # Environment variables (NOT committed)
├── requirements.txt
└── README.md
```

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
```

### 2. Install dependencies

```bash
python3 -m venv .venv
source .venv/bin/activate   # On Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

### 3. Generate your own keys

Never commit your private key!

Generate a 2048-bit RSA key pair:

```bash
openssl genrsa -out keys/private.pem 2048
openssl rsa -in keys/private.pem -pubout -out keys/public.pem
```

- `keys/private.pem` → **Keep secret**, used for signing.
- `keys/public.pem` → Safe to share, used for verification.

### 4. Configure environment variables

Create a `.env` file in the project root:

```
PRIVATE_KEY_PATH=keys/private.pem
PUBLIC_KEY_PATH=keys/public.pem
```

`.env` is already in `.gitignore` so it won’t be committed.

### 5. Generate a certificate + QR

```bash
python main.py --json sample.json 
```

This will:
- Sign the certificate JSON with your private key.
- Embed `{ cert, sig }` into the PDF metadata `/CertPayload`.
- Generate a QR code PNG via `qr_utils.py` for verification (for browser verifier use).
- Add the QR code in the signed pdf and save the pdf in `{/out}`.

### 6. Verify a certificate offline

```bash
python verifier.py --pdf out/certificate.pdf
```

**Expected output:**
```
✅ Certificate is VALID
{
  "performer": { ... },
  "media": { ... },
  "sanitization": { ... },
  "destination": { ... }
}
```

If invalid:

```
❌ Certificate verification FAILED
```

---

## 📄 License

See [LICENSE](LICENSE) for details.

---

## 🙋‍♂️ Questions?

Feel free to open an [issue](https://github.com/<your-username>/<your-repo>/issues) or contact the repo owner for help.
