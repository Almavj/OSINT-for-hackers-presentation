# OSINT for Hackers

**Reconnaissance, Exposure, and Defense** — a 17-slide training deck on Open-Source
Intelligence for offensive and defensive security practitioners.

Passive collection · Attack-surface mapping · Investigator tradecraft

| | |
|---|---|
| **Format** | PowerPoint (16:9) |
| **Slides** | 17 |
| **Also included** | PDF export for quick viewing |
| **Audience** | Penetration testers, red teamers, SOC analysts, security engineers, and blue-team defenders |

> **Read this first.** Every technique in this deck is for **authorized** security
> testing and defensive use only. See [Legal & ethical use](#legal--ethical-use).

---

## Why this deck exists

Attackers map your public footprint before they ever touch your network. This deck
covers the same collection tradecraft so defenders can find the exposure first, prove
it in a report, and fix it. OSINT is treated here as **structured analysis**, not as
a list of tools.

---

## Contents

| # | Section | Covers |
|---|---|---|
| 1 | Title | Scope and framing |
| 2 | Agenda | The six modules that follow |
| 3 | **Foundations** | What qualifies as OSINT, the key rule, and the five things pre-engagement recon identifies |
| 4 | **Methodology** | A repeatable five-step model: Define → Scope → Collect → Validate → Report |
| 5 | **Search-engine OSINT** | Google dork operators table (`site:`, `inurl:`, `intitle:`, `filetype:`, `cache:`, `-`) |
| 6 | **Search-engine OSINT** | Document & image metadata — `exiftool` extraction, GPS, author names, internal paths |
| 7 | **Infrastructure OSINT** | Domain & DNS discovery — WHOIS/RDAP, reverse WHOIS, certificate transparency, `dig`, ASN ownership |
| 8 | **Infrastructure OSINT** | Subdomain enumeration — passive (`crt.sh`, `amass`, `subfinder`) vs. active (`dnsx`, `httpx`) |
| 9 | **Infrastructure OSINT** | Shodan, Censys, and GreyNoise — example queries for exposed services and banners |
| 10 | **Infrastructure OSINT** | Technology fingerprinting, Wayback Machine / passive DNS history, code-exposure searches |
| 11 | **Human OSINT** | People & email discovery — `theHarvester`, Hunter.io, HIBP, email-format permutations |
| 12 | **Human OSINT** | Username & social-media correlation — Sherlock, Maigret, WhatsMyName, Maltego |
| 13 | **Image, archive & physical** | EXIF/GPS, reverse image search, web archives, Street View and geo sources |
| 14 | **Tooling** | Essential tools by category, each mapped to the question it answers |
| 15 | **Investigator tradecraft** | A six-step OPSEC workflow for protecting investigator, client, and findings |
| 16 | **Deliverable** | Reporting structure — from executive summary to risk matrix and appendix |
| 17 | **Closing** | Key takeaways |

---

## The methodology in brief

The deck's spine is a five-step loop. Skip it and you are just running tools.

| Step | Name | Output |
|---|---|---|
| 1 | **Define** | The intelligence requirement — what question is being answered |
| 2 | **Scope** | Org, domains, IP ranges, geography, time window, authorization |
| 3 | **Collect** | Passive DNS, certs, search engines, repos, social media, archives |
| 4 | **Validate** | Cross-verify across independent sources; assign a confidence level |
| 5 | **Report** | Attack-surface map, evidence, risk rating, remediation |

## Key rule

> If obtaining the information requires **authentication, exploitation, or
> circumventing a control**, it is no longer OSINT.

OSINT ends exactly where authentication, exploitation, or unauthorized access begins.

---

## Tool reference

| Category | Tools | Question answered |
|---|---|---|
| Domains | Amass, Subfinder, theHarvester, crt.sh | What external assets exist? |
| Infrastructure | Shodan, Censys, GreyNoise | What services & devices are exposed? |
| Relationship mapping | Maltego, SpiderFoot, Recon-ng | How do entities connect? |
| People / usernames | Sherlock, Maigret, WhatsMyName | Where does this identity appear? |
| Email / breaches | Hunter.io, Have I Been Pwned, holehe | What contact & exposure indicators exist? |
| Images | ExifTool, Google Lens, TinEye | What metadata or visual context exists? |
| Archives | Wayback Machine, archive.today | What existed previously? |

### Passive vs. active subdomain enumeration

- **Passive** — no contact with the target. Certificate transparency, public DNS
  datasets, search engines, archives, third-party data.
  ```bash
  curl -s "https://crt.sh/?q=%25.example.com&output=json" | jq -r '.[].name_value'
  amass enum -passive -d example.com
  subfinder -d example.com -silent
  ```
- **Active** — interacts with the target. **Authorization required.**
  ```bash
  dnsx -l subdomains.txt -silent
  httpx -l live-hosts.txt -title -tech-detect -status-code
  ```

### High-value patterns to flag

`dev` / `test` / `staging` / `uat` / `demo` · `vpn` / `mail` / `remote` / `portal` /
`admin` / `api` · forgotten cloud-hosted apps · **dangling CNAMEs** (subdomain
takeover) · domains resolving to default pages · expired or misconfigured DNS.

---

## Legal & ethical use

This material is provided for **defensive security, authorized penetration testing,
and security education**.

- Only collect and test against systems you **own or have explicit written
  permission** to assess.
- Follow the laws of your jurisdiction. OSINT collection can still breach
  computer-misuse, data-protection (e.g. GDPR), privacy, and copyright law.
- Respect platform terms of service, `robots.txt`, and rate limits.
- Do not use collected personal data for harassment, doxxing, stalking, or
  unsolicited outreach.
- Findings about real organizations should go through the responsible disclosure
  process, not into a public repository.

The authors are not responsible for misuse of this material. Anything requiring
authentication, exploitation, or control circumvention is out of scope — and is not
OSINT.

---

## Files

```
OSINT_for_Hackers.pptx   Source deck (editable, 17 slides, 16:9)
OSINT_for_Hackers.pdf    PDF export for viewing and printing
README.md                This file
LICENSE                  MIT
```

## Viewing the deck

- **PowerPoint / Keynote / Google Slides** — open `OSINT_for_Hackers.pptx` directly.
- **Anything else** — open `OSINT_for_Hackers.pdf`.

To regenerate the PDF from the source deck:

```bash
soffice --headless --convert-to pdf OSINT_for_Hackers.pptx
```

The deck has speaker notes on all 17 slides.

---

## Contributing

Found an error, a stale tool name, or a gap in the methodology? Pull requests are
welcome.

1. Fork the repository
2. Create a branch: `git checkout -b fix/short-description`
3. Make your change to `OSINT_for_Hackers.pptx` (or the README)
4. Commit and open a pull request describing what changed and why

For substantive changes, please keep the existing scope discipline — techniques that
require authentication or exploitation belong in a different kind of document.

## License

Released under the [MIT License](LICENSE). See the `LICENSE` file for full terms.
