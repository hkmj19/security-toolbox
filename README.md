# HKMJ Tools

Small browser tools for security work, debugging, and everyday data inspection.

**Website:** [projects.hemanthkumarmj.com](https://projects.hemanthkumarmj.com/)

The homepage is a searchable tools directory. Every tool is a **self-contained HTML file** with its CSS and JavaScript embedded. Double-click a file to use it; no server, assets folder, package installation, or build step is needed. The pages share a dark security-tool theme and responsive layouts.

There are no accounts, ads, analytics, external fonts, or third-party JavaScript dependencies. DorkSec and the Telegram checker need internet access for their external services; the other tools process input locally.

## Tools

| Tool | What it does | Where input goes |
| --- | --- | --- |
| [Hash format inspector](hashAnalyzer.html) | Suggests possible hash families from the character set, digest length, and markers such as bcrypt, Argon2, and crypt prefixes. It does not crack hashes or prove which algorithm was used. | Processed locally |
| [File checksum](fileChecksum.html) | Calculates SHA-256, SHA-384, or SHA-512 for a selected file and optionally compares the result with an expected hexadecimal digest. Supports files up to 100 MiB. | Processed locally; no upload |
| [IPv4 subnet calculator](ipv4SubnetCalculator.html) | Calculates masks, network boundaries, address ranges, usable host counts, and binary representations. Accepts CIDR, dotted netmasks, wildcard masks, and requested host counts. | Processed locally |
| [Base64 & URL encoder](textEncoder.html) | Encodes and decodes UTF-8 text as Base64, Base64url, or a URL component. Supports Unicode text and rejects invalid encoded input. | Processed locally |
| [JSON formatter](jsonFormatter.html) | Validates JSON and produces either indented or minified output. Parse errors appear next to the input/output workspace. | Processed locally |
| [JWT inspector](jwtInspector.html) | Decodes a JWT header and payload, then displays `iat`, `nbf`, and `exp` in UTC. Signatures, issuers, audiences, and permissions are **not verified**. | Processed locally |
| [Timestamp converter](timestampConverter.html) | Converts Unix seconds, Unix milliseconds, and ISO 8601 dates with explicit timezone offsets. Shows UTC and the device’s local time. | Processed locally |
| [Unicode lookalike explorer](homoglyph.html) | Lets you choose visually similar characters and inspect the resulting Unicode code points. The character set is curated; output is not checked for domain registration or IDNA validity. | Processed locally |
| [DorkSec](dorksec.html) | Launches domain-focused searches and external DNS, certificate, archive, and technology lookup services. These are shortcuts, not an automated vulnerability scan. | Sent to the external service you select |
| [Telegram bot checker](telegramBotApiChecker.html) | Calls Telegram’s `getMe` endpoint with your own bot token and shows the returned bot identity. It does not read chats or audit a bot. | Sent directly to `api.telegram.org` |

## Examples

- **Subnet:** enter `192.168.1.130/26` to get network `192.168.1.128`, upper address `192.168.1.191`, and 62 usable host addresses. The equivalent netmask is `255.255.255.192`.
- **Checksum:** choose a downloaded file, select the algorithm used by its publisher, and paste their trusted checksum to compare the bytes.
- **Encoding:** select “Text → Base64,” enter UTF-8 text, then use “Base64 → Text” to decode it. Encoding is not encryption.
- **JSON:** paste an API response and choose pretty print or minify. Invalid JSON produces a parse error rather than partial output.
- **JWT:** paste a three-segment token to read its claims. Treat every displayed claim as untrusted until the token is verified by the appropriate system.
- **Time:** `1794139200` seconds, `1794139200000` milliseconds, and `2026-11-08T17:30:00+05:30` all represent `2026-11-08T12:00:00.000Z`.

## Local use

Use a current browser with JavaScript enabled. Open `index.html` directly, or copy any individual tool `.html` file elsewhere and open it. Its styling and logic travel with the file.

File hashing uses Web Crypto. It works in current Chrome when opening a local HTML file; if another browser blocks it, use HTTPS or localhost. Clipboard buttons may require browser permission; the output can always be copied manually.

## Validation

During development, the tools passed 18 logic and form-handler tests, 12 browser checks, and direct-file checks for all 11 HTML pages. Mobile checks covered a 320px viewport. Direct-file checks used copies of the HTML files in a folder without supporting assets.

Telegram API responses were mocked during these checks; this does not verify a live bot token or guarantee external-service availability. Development test scripts are not included in this standalone distribution.

## Editing the files

Each HTML page contains its own `<style>` and `<script>` sections. Edit these directly to change the appearance or behavior. Shared design changes need to be applied to the relevant pages. There is no required build step or separate asset dependency.

## Input handling and limits

- Local tools do not upload or persist your input. There is no application backend, local storage, or telemetry. The host still receives ordinary page requests, and browser extensions can affect page privacy.
- DorkSec sends the entered domain to the selected service. Some links launch active checks. Use domains you own or have permission to assess.
- Telegram requires the token in the API request URL. This page masks the field, does not save it, and sends the request directly to Telegram. Clear it when finished and keep browser/network logs private.
- File hashing reads the whole file into memory, so the browser tool limits input to 100 MiB. A matching checksum does not establish that a file is safe.
- JSON parsing follows JavaScript number precision. Keep large integer identifiers as strings; duplicate keys retain their last value.
- JWT inspection does not authenticate tokens. Time comparisons use the device clock. Encrypted JWE tokens are unsupported.
- Hash-family detection is heuristic. Unicode lookalikes depend on fonts and do not necessarily form valid domain names.

## Repository layout

```text
index.html                  Searchable tools directory
hashAnalyzer.html           Hash format inspector
fileChecksum.html           File hashing and checksum comparison
ipv4SubnetCalculator.html   IPv4 network calculations
textEncoder.html            Base64, Base64url, and URL encoding
jsonFormatter.html          JSON validation and formatting
jwtInspector.html           JWT decoding without signature verification
timestampConverter.html     Unix and ISO timestamp conversion
homoglyph.html              Unicode lookalike exploration
dorksec.html                External domain-research shortcuts
telegramBotApiChecker.html  Telegram getMe token check
README.md                   Tool descriptions and usage
CNAME                       Custom domain for GitHub Pages
```

The site is static and keeps its existing GitHub Pages routes and custom domain. Deploy the repository root through GitHub Pages; no generated `dist` folder is needed.

Maintained by [Hemanth Kumar M J](https://github.com/hkmj19).
