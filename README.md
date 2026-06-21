# Fireweed Compliance Site

Static compliance page for **Fireweed Cannabis of Vermont** (Vermont CCB License #SCLT0457).
Serves the disease-free guarantee, disease mitigation / IPM SOP, and per-lot pesticide test
results required on vegetative plant labels by CCB Vegetative Plant Sales Guidance.

Target URL: **https://compliance.fireweedcannabisofvermont.com**
This is the URL encoded in the label QR code (`assets/compliance-qr.svg`).

---

## Structure

```
fireweed-compliance/
├── index.html        # the entire page — self-contained, no build step, no dependencies
├── docs/             # compliance PDFs (test results + SOP) — see docs/README.txt
└── assets/
    ├── compliance-qr.svg   # QR for the label (vector — use this for print)
    └── compliance-qr.png   # QR for screen / proofing
```

There is no build step. What's in the repo is what's served.

---

## Deploy to Cloudflare Pages (free tier)

1. Push this folder to a Git repo (GitHub/GitLab).
2. Cloudflare dashboard → **Workers & Pages → Create → Pages → Connect to Git**.
3. Select the repo. Build settings: **Framework preset = None**, **Build command = (blank)**,
   **Build output directory = /** (root). Save and deploy.
4. **Custom domain:** Pages project → Custom domains → add
   `compliance.fireweedcannabisofvermont.com`. Cloudflare provisions SSL automatically.
   - Smoothest if the domain's DNS is on Cloudflare (move nameservers at GoDaddy — this is a
     nameserver change, not a domain transfer, and leaves the apex marketing site untouched).
   - If DNS stays at GoDaddy, add the CNAME Cloudflare shows you for the subdomain.

After this, every `git push` redeploys. The printed QR never changes because it points at the
domain root, not at any per-lot path.

---

## Adding a new pesticide test result

1. Drop the lab COA PDF in `docs/`, named `[LOTNUMBER]_pesticide-test.pdf`
   (e.g. `APFRT-C-2026-001_pesticide-test.pdf`).
2. In `index.html`, in the results table, either:
   - update the existing row's last cell from the "Test pending" badge to
     `<a class="testlink" href="docs/SRCHM-C-2026-001_pesticide-test.pdf">View COA</a>`, or
   - add a new `<tr>` for a new lot following the same pattern.
3. Commit and push.

## Posting the SOP

Drop the SOP PDF in `docs/` as `fireweed-disease-mitigation-sop.pdf`, then in the "Procedures"
section of `index.html` change the `<a class="doc pending">` element into a real link
(`href="docs/fireweed-disease-mitigation-sop.pdf"`) and remove the `pending` class and
`aria-disabled`.

---

## Notes

- Page is intentionally dependency-free so it always renders, even if a CDN is unreachable.
- No personal address appears anywhere in this repo (by design — see the open task on business
  structure / address protection).
- QR: version 4, error-correction level M. Regenerate only if the target URL changes.
EOF
