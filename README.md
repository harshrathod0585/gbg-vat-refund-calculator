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

Adds a currency selector to the calculator on
[Gangnam Beauty Guide](https://gangnambeautyguide.com/en/tools/tax-refund/), which quotes only in
won and USD. Enter your quote in any of 23 currencies instead of converting it on Google first.
Switching currency carries the value across rather than reusing the digits.

VAT-inclusive quotes are divided by 11, not multiplied by 0.10 — the tax is already inside the
number, so on a ₩5,000,000 quote the difference is ₩45,455.

## Design decisions

**Exchange rates are frozen in at build time, not fetched live.** A static page cannot be down;
a live API call can fail in front of the reader. The rate carries a visible "as of" date.

**Rounded quick-pick amounts.** The chips convert the common won quotes into the selected currency
and round to something a person would actually type, rather than showing $3,735.42.

## Note on the refund itself

Korea abolished this scheme on 31 December 2025
([Korea Herald](https://www.koreaherald.com/article/10633779),
[Korea Biomedical Review](https://www.koreabiomed.com/news/articleView.html?idxno=28501)).
The page states that up front and is framed as estimating the scheme as it applied up to that
date, rather than implying a refund is currently available.

## Verification

`compute()` is pure and a self-check runs on load — open the console. It asserts the VAT-inclusive
divisor, the 70% net refund, the 31 December 2025 boundary in both directions, the blank-date
guard, and that currency conversion happens before the tax maths.

Single file, no dependencies, no build step.
