# Korea VAT Refund Calculator — 2026 rules

Live: https://gbg-vat-refund.vercel.app

A replacement for the tax refund calculator on
[Gangnam Beauty Guide](https://gangnambeautyguide.com/en/tools/tax-refund/), which tells
foreign patients they can reclaim "up to 10% of their surgery costs."

Korea abolished that scheme on 31 December 2025. The National Assembly omitted the relevant
clauses from the revision of the Act on Restriction on Special Cases Concerning Taxation, and
no refund is available for procedures performed from 1 January 2026
([Korea Herald](https://www.koreaherald.com/article/10633779),
[Korea Biomedical Review](https://www.koreabiomed.com/news/articleView.html?idxno=28501)).

## What it does

- **Surgery date decides eligibility.** On or before 31 December 2025 it shows the refund and
  reproduces the incumbent calculator's figures exactly. From 1 January 2026 it shows the
  non-refundable VAT and what you actually pay.
- **23 currencies.** Enter your quote in your own money instead of converting on Google.
  Switching currency carries the value across rather than reusing the digits.
- **VAT-inclusive quotes are divided by 11, not multiplied by 0.10.** Korean clinics often quote
  pre-VAT, and on a ₩5,000,000 quote the difference is ₩45,455.

## Design decisions

**Exchange rates are frozen in at build time, not fetched live.** A static page cannot be down;
a live API call can fail in front of the reader. The rate carries a visible "as of" date.

**A blank date is never eligible.** `"" <= "2025-12-31"` is true in JavaScript, so a cleared date
field would otherwise report a claimable refund — the same class of error this page exists to fix.

**No refund shows the VAT you absorb, not a zero.** A grey ₩0 is true but useless; the number
that still changes hands is the one worth showing.

## Verification

`compute()` is pure and a self-check runs on load — open the console. It asserts the VAT-inclusive
divisor, the 70% net refund, the 31 December 2025 boundary in both directions, the blank-date
guard, and that currency conversion happens before the tax maths.

Single file, no dependencies, no build step.
