# Securevault
🔐 Server-side encrypted file vault built with Python &amp; Flask — AES encryption, SHA-256 integrity verification, and brute-force protection.

# 🔐 SecureFileVault
### BTech Cybersecurity Project — Server-Side AES Encryption

---

## Project Structure

```
SecureVaultServer/
│
├── app.py              ← Flask web application (routes, logic)
├── crypto_utils.py     ← Cryptographic functions (encrypt, decrypt, hash)
├── requirements.txt    ← Python dependencies
├── vault_meta.json     ← Auto-created: file metadata store
│
├── templates/
│   ├── base.html       ← Shared layout, styles, navigation
│   ├── index.html      ← Vault file list
│   ├── upload.html     ← Encrypt & upload page
│   ├── download.html   ← Decrypt & download page
│   └── logs.html       ← Activity log viewer
│
├── uploads/            ← Stores encrypted .vault files
└── logs/
    └── vault.log       ← All vault activity logged here
```

---

## Setup & Run (Windows + VS Code)

### Step 1 — Install Python
Make sure Python 3.9+ is installed. Check with:
```
python --version
```

### Step 2 — Create Virtual Environment (recommended)
```
cd SecureVaultServer
python -m venv venv
venv\Scripts\activate
```

### Step 3 — Install Dependencies
```
pip install -r requirements.txt
```

### Step 4 — Run the App
```
python app.py
```

### Step 5 — Open in Browser
```
http://127.0.0.1:5000
```

---

## How to Use

1. **Encrypt a File**
   - Go to "Encrypt" in the navbar
   - Select a file and enter a password (min. 6 chars)
   - Click "Encrypt & Store"

2. **Download / Decrypt a File**
   - Go to "Vault" → click "⬇ Decrypt" next to a file
   - Enter the correct password
   - File is downloaded after integrity check passes

3. **Brute-Force Protection**
   - 3 wrong passwords → file is permanently **locked**
   - Locked state is saved in vault_meta.json

4. **View Logs**
   - Click "Logs" in the navbar to see all vault activity

---

## Security Concepts

### AES (Advanced Encryption Standard)
A symmetric-key block cipher — the same key encrypts and decrypts.
We use Fernet which implements AES-128-CBC with random IV per file.

### HMAC-SHA256
Fernet also attaches a Message Authentication Code (MAC) to every
encrypted token. Any bit change in the ciphertext will cause
decryption to fail, protecting against tampering.

### SHA-256 Key Derivation
User password → SHA-256 hash → 32-byte AES key.
This ensures the key is always the right length regardless of password.

### SHA-256 Integrity Hash
We hash the plaintext file at upload time and store the hex digest.
After decryption, we re-hash and compare. Matching = file is intact.

### Server-Side Encryption
The plaintext file is encrypted AFTER arrival on the server.
The server generates and applies the key — the browser never sees it.
Only the ciphertext (.vault file) is persisted on disk.

### Brute-Force Protection
Failed password attempts per file are tracked in vault_meta.json.
After 3 failures, the file entry is locked — no further attempts allowed.

---

## Requirements

- Python 3.9+
- flask >= 2.3.0
- cryptography >= 41.0.0
- werkzeug >= 2.3.0

