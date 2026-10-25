# Cyber Suraksha Gram (साइबर सुरक्षा ग्राम)

A bilingual (Hindi / English) mobile-first web app that teaches rural and first-time smartphone users in India how to recognise and avoid common online frauds. A friendly turbaned guide, **Suraksha Dada**, walks the learner through short, one-screen lessons.

The whole app is a single self-contained file, `index.html`. There is no build step, no server and no dependencies.

## Features

The app is a guided journey of 8 steps, shown with a progress bar at the top:

1. **Login** – Enter your name and village. No password is needed. Details are kept only on your device.
2. **Welcome** – Three golden rules (never share OTP/PIN, never type a PIN to receive money, stop when you feel fear or greed) and an overview of the lessons.
3. **Spot scams** – Five common scams, each with a sample scammer message, red flags and what to do:
   - Fake KYC phone call
   - "Money sent by mistake" on UPI
   - Lottery or prize
   - Fake job offer
   - Fake bank link / `.apk` file
4. **Check SMS** – Paste an SMS or WhatsApp message and get a verdict: *Dangerous*, *Be careful* or *Looks safe*, with the reasons and next steps. Three sample messages are included. The check runs entirely in the browser; nothing is sent anywhere.
5. **Do and Don't** – Expandable cards for OTP, PIN and password, UPI, social media, public Wi-Fi and fake apps.
6. **Quiz** – 10 multiple-choice questions with explanations. The score earns a badge:
   - 9-10: Cyber Rakshak (gold)
   - 6-8: Cyber Mitra (silver)
   - 0-5: Learning Saathi (bronze)
7. **Help** – The cyber crime helpline **1930** (tap to call), the cybercrime.gov.in portal link, a 5-step checklist for what to do if money is lost, and a share block.
8. **Feedback** – Star rating, what you liked, an optional comment, then a completion certificate with your name, village and badge.

### Accessibility and usability

- Hindi and English, switchable at any time from the header
- "Listen" button on the guide reads the message aloud (browser speech synthesis, hi-IN / en-IN)
- Large touch targets, high contrast, visible focus outlines
- Light and dark theme following the system setting
- Respects reduced-motion preferences and device safe areas (notches)
- Share via WhatsApp or copy the link

## How to run

Open `index.html` in any modern browser, or host it as a static page (GitHub Pages, Netlify, any web server). For best results on a phone, open it in Chrome.

An internet connection is only needed for the Google Fonts (Baloo 2 and Hind). Without it, the app falls back to system fonts.

## Privacy

- No accounts, analytics or backend.
- `localStorage` stores only your language, name and village (`csg`) and your submitted feedback (`csg-feedback`). It never leaves the device.
- Pasted messages in the SMS checker are analysed locally and are not stored.

## How the SMS checker works

The checker in `analyze()` is a rule-based scorer, not AI. Each rule is a regular expression with a weight:

| Signal | Examples |
| --- | --- |
| OTP, PIN, CVV, password mentions | "send the OTP" |
| Urgency | "tonight", "immediately", "अभी" |
| Account-block threats | "account will be blocked" |
| KYC bait | "update KYC" |
| Prize or lottery | "you have won" |
| Fees | "processing fee" |
| Money requests | "send ₹5,000", "by mistake" |
| Short links | bit.ly, tinyurl |
| `.apk` files, remote-access apps | AnyDesk, TeamViewer |
| Easy-job promises, secrecy requests | "work from home" |
| Unknown links or phone numbers to call | non-official domains |

Scoring: `.apk`, OTP, or remote-access apps trigger *Dangerous* immediately; a total of 5 or more is *Dangerous*, 2-4 is *Be careful*, below 2 is *Looks safe*. Messages that themselves warn "do not share OTP" (typical genuine bank texts) are not penalised for mentioning OTP. Official domains (gov.in, major banks) are treated as a good sign.

**Limitation:** this is only a helper. A "safe" result is not a guarantee, and the app tells users to ask an elder or their bank when unsure.

## Customising

Everything lives in `index.html`:

| To change | Edit |
| --- | --- |
| Lesson steps | `STEPS` |
| Scam stories | `SCAMS` |
| Do and Don't topics | `DOS` |
| Quiz questions and answers | `QZ` (`a` is the index of the correct option) |
| Recovery checklist | `HELP_STEPS` |
| Checker rules and samples | `RULES`, `SAMPLES` |
| Colours and fonts | CSS variables in `:root` |

All text is stored as `[Hindi, English]` pairs. Keep both when adding content. To add a language, extend the `T()` and `P()` helpers.

## Tech notes

- Plain HTML, CSS and vanilla JavaScript (ES5 style)
- Inline SVG icons, illustrations and badges
- State kept in a single object `S`; views are functions in `V` rendered into `#stage`
- Event handling by delegation on `data-a` attributes

## Important contacts (India)

- Cyber crime helpline: **1930**
- Report online: **cybercrime.gov.in**
- Your bank's helpline is printed on the back of your card.

Please verify these numbers against official sources before distributing the app, as they may change.
