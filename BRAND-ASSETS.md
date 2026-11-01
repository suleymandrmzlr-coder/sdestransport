# SDES Logistics visual refresh

Generated using the built-in image generation tool. Final website assets:

- `/workspace/sites/sdes-logistics/dist/assets/sdes-hero.png`: Cinematic photorealistic CGI of an SDES-branded white European tractor and charcoal curtain-side trailer at a blue-hour international freight terminal; navy, white and burnt orange; negative space for page typography.
- `/workspace/sites/sdes-logistics/dist/assets/sdes-trailers.png`: Photorealistic CGI of SDES-branded curtain-side and refrigerated trailers at a logistics terminal, with physically plausible equipment and navy/orange lighting.
- `/workspace/sites/sdes-logistics/dist/assets/sdes-logo.png`: Bold italic white SDES LOGISTICS wordmark, orange interlocking directional S emblem and orbital arc on deep navy; refined to remove texture defects.

The website labels these generated scenes as brand concepts, not evidence of fleet or warehouse ownership. Logo exploration was refined once for clean edges and solid lettering. All final assets are included in the site source repository.

## Interaction and scope

- Dependency-free canvas globe with 3D projection, stylized coastlines, representative connections, region buttons, drag rotation and a pause control. Not live tracking or navigational mapping.
- CSS perspective cards, dimensional brand scene and subtle pointer movement. Respects reduced motion.
- Responsive navigation, service menu, corporate content, equipment, regions, carrier partnership, logistics guide, FAQ, privacy information and contact section.
- Existing local-only quote summary creation, copying and text download retained. No backend submissions or invented contact details.

## Verification

HTML anchors, image assets, unique IDs, image descriptions and JavaScript syntax checked. Browser automation unavailable in the active environment; no browser rendering claim is made.

## International site update

- English is the default entry (`index.html`); Spanish (`es.html`) and Turkish (`tr.html`) are complete static language variants. `source/build-locales.py` builds all three from the Turkish source and translation files.
- Official third-party logos are stored locally; provenance URLs are in `source/brand-sources.json`. The carousel is explicitly an industry showcase, not an unverified endorsement or customer reference claim. No additional customer relationships were invented.
- Documentation badges are generic, subdued category icons. The site does not claim SDES certification or permit ownership.
- LinkedIn, Instagram, Facebook and YouTube icons show a coming-soon notice until the actual account URLs are provided.
- WhatsApp assistant prepares translated messages and incorporates enquiry details. Sending is disabled until an actual international phone number is supplied. Configure verified details in `dist/contact-config.js`; no number or social account was guessed.
- Three locale pages validated for translated text, existing anchors/assets, stable form option values and script load order. JavaScript syntax checked. Full browser QA unavailable.
