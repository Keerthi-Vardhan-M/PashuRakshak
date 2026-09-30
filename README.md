# PashuRakshak

**Live website:** [https://pashurakshak.pages.dev/](https://pashurakshak.pages.dev/)

PashuRakshak helps farmers and field workers record signs of illness in cattle and buffalo, then gives a clear priority level for veterinary review. It keeps local case records, highlights possible disease clusters, and supports a veterinarian-led feedback loop.

## What the prototype does

- Records a sick-animal report with animal details, village, symptoms, vaccination status, and an optional photograph.
- Supports English, Hindi, and Marathi labels for the reporting flow.
- Uses explainable symptom-based rules to classify a report as Low, Moderate, High, or Emergency priority.
- Flags possible Foot-and-Mouth Disease (FMD), Lumpy Skin Disease (LSD), and Haemorrhagic Septicaemia (HS) from the selected signs.
- Shows the reasons behind a risk result and a recommended next step.
- Provides a veterinarian work queue where pending cases can be verified or referred.
- Shows a seven-day outbreak radar for similar high-risk FMD reports across nearby villages.
- Maintains local animal health records and exports veterinarian-reviewed feedback as JSON.
- Works as an installable Progressive Web App (PWA) and caches core files for offline use.

## Current architecture

```text
Farmer / Field Worker
        ↓
PashuRakshak reporting form
        ↓
Explainable rule-based triage
        ↓
Priority result and immediate guidance
        ↓
Veterinarian work queue
        ↓
Verification, referral, follow-up, and feedback export
        ↓
Outbreak radar and local health records
```

The current repository is a self-contained frontend prototype. It runs entirely in the browser and stores demo reports locally on the device.

## Technology used

| Area | Current implementation |
| --- | --- |
| UI | HTML, CSS, and vanilla JavaScript |
| Offline support | Service Worker and Cache Storage |
| Local reports | `localStorage` |
| Uploaded photographs | IndexedDB |
| PWA metadata | Web App Manifest |
| Triage | Explainable, deterministic symptom-weight rules |

## Run locally

Use a local web server so the service worker and offline features can work correctly.

### Option 1: VS Code Live Server

1. Open this folder in Visual Studio Code.
2. Install the **Live Server** extension if needed.
3. Right-click `index.html` and select **Open with Live Server**.

### Option 2: Node.js

```bash
npx serve .
```

Open the local URL printed in the terminal, usually `http://localhost:3000`.

## How triage works

The prototype evaluates only the signs selected by the user. It does not use a trained model and it does not provide a confirmed medical diagnosis.

| Possible concern | Examples of weighted signs |
| --- | --- |
| FMD | Mouth or tongue blisters, hoof lesions, salivation, lameness, fever |
| LSD | Firm skin nodules, swollen lymph nodes, limb or udder swelling, fever |
| HS | Jaw or neck swelling, breathing difficulty, frothing at the mouth, nasal discharge |

Priority is determined from the symptom score. Breathing difficulty with neck swelling or frothing raises an Emergency result. The app also increases risk slightly when vaccination is not current.

The outbreak radar becomes active when it finds at least five high-risk FMD reports across at least two villages during the previous seven days. This remains a preliminary signal until a veterinarian reviews the cases.

## Repository structure

```text
.
├── index.html      # Complete application UI, styles, rules, and interactions
├── manifest.json   # PWA metadata
├── sw.js           # Service worker and offline cache logic
└── icon.svg        # App icon
```

## Important limitations

- This is a prototype for demonstration and early workflow validation.
- Risk labels are decision-support cues. A qualified veterinarian must verify every suspected case before treatment or escalation.
- Case records and photographs remain only in the browser on the current device. They are not shared between devices.
- The outbreak radar uses demonstration data and a simple threshold. It does not calculate real geographic distance or connect to official disease-surveillance systems.
- The veterinarian sign-in screen is a client-side demo control. It is not a production authentication system.

## Planned next steps

- Move case storage from browser storage to a local Java Spring Boot backend with SQLite.
- Add real user roles and secure veterinarian authentication.
- Add verified veterinary actions, treatment records, and follow-up scheduling.
- Replace the demonstration radar with village and district mapping based on actual location data.
- Add safe synchronisation between local field deployments when connectivity is available.
- Integrate with approved veterinary reporting workflows where permitted.

## Safety note

PashuRakshak supports early reporting and case prioritisation. It must not be used as a substitute for veterinary diagnosis, laboratory confirmation, or official outbreak reporting procedures.
