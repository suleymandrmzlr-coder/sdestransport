# SDES Hostinger CMS — continuation rules

Read README.md, SON-DURUM.md and HOSTINGER-KURULUM.md first. This is an independent Node.js/MariaDB migration, not a Cloudflare Worker. Preserve the SDES visual identity, accepted logistics workflow and source data. Do not deploy to the original Sites project or change DNS without the user asking.

English is default; preserve Spanish and Turkish, support German and additional locales with explicit English fallback until translated. All visitor interface strings belong in the CMS dictionaries or editable page fields. Preserve data-cms-node attributes and existing field keys; never renumber them.

Authorization is server-side. Never trust client permissions or oai-authenticated headers. No hardcoded credentials. Do not expose private load addresses, keys, references, plates or customer/carrier details in public cards. Preserve hashed tracking keys, document snapshots, soft deletion and delivery/customer totals.

Use npm run build and npm test. Tests use an isolated SQLite compatibility database; real MariaDB deployment verification is separate. Do not claim unperformed mobile or live-hosting tests. Public launch also needs real company/contact details and final privacy/legal review.

No mandatory paid services, invented certifications, references, fleet ownership or unverified company claims. Preserve unchanged production data. Future package releases must exclude secrets, node_modules and temporary preview fixtures.
