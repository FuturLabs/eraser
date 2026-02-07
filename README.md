# eraser

Email sanitizer that runs entirely in your browser. Upload a `.eml` or `.txt` email file and eraser strips out personal information, keeping only the anonymized template structure.

## How it works

1. **Parse** the email's MIME structure (headers, multipart boundaries, content-transfer-encoding)
2. **Strip** non-essential headers — only `From`, `Subject`, `Content-Type`, and `Content-Transfer-Encoding` are kept
3. **Redact** personally identifiable information using pattern matching
4. **Sanitize** HTML content (neutralize tracking links, replace images with placeholders)
5. **Present** the cleaned result for review, manual editing, and export

## What gets redacted

| Data type | Placeholder | Example |
|---|---|---|
| Dollar amounts | `[AMOUNT]` | `$1,234.56` |
| Email addresses | `[EMAIL]@domain.com` | Local part removed, domain kept |
| Phone numbers | `[PHONE]` | 7-11 digit patterns |
| SSNs | `[SSN]` | `123-45-6789` |
| Credit card numbers | `[CARD_NUMBER]` | 16-digit card patterns |
| Partial card numbers | `[LAST4]` | "ending in 1234", "****5678" |
| Long number sequences | `[ACCOUNT_NUM]` | 6-17 digit sequences |
| Street addresses | `[ADDRESS]` | "123 Main Street" |
| State + ZIP | `[STATE] [ZIP]` | "CA 90210" |
| IP addresses | `[IP_ADDRESS]` | `192.168.1.1` |
| Names | `[NAME]` | Detected via greetings, honorifics, labeled fields, display names |
| Sender names | `[SENDER_NAME]` | From header display name |

## Features

- **100% client-side** — no data leaves your device, no server, no dependencies
- **MIME support** — handles multipart emails, quoted-printable, base64, and RFC 2047 encoded headers
- **HTML-aware redaction** — parses HTML with DOMParser and walks text nodes so tags are never broken
- **Email preview** — rendered HTML in a sandboxed iframe
- **Click-to-redact** — click any text in the preview to manually redact it
- **Raw source editing** — switch to a monospace view with highlighted placeholders, or edit directly
- **Export** — copy to clipboard or download as `.txt`
- **Drag-and-drop** — drop a file or use the file picker (`.eml` and `.txt`, max 10MB)

## Usage

Open `eraser.html` in any modern browser. No build step, no install, no server required.

## Privacy

Everything runs in the browser via the FileReader API. The email file is never uploaded anywhere. The HTML preview iframe uses `sandbox="allow-same-origin"` to prevent script execution in rendered email content.
