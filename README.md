# Gold Fit Gym

A responsive, multi-page website for a fictional Bucharest gym, built with semantic HTML5, CSS3 and vanilla JavaScript. No frameworks, no build step, no dependencies.

Gold Fit Gym is a concept brand. The project covers the front end of a full gym presence: marketing pages, a service catalogue, a live subscription price calculator, a coaches directory loaded from JSON, a validated contact form and a client-side login flow.

The site content is in Romanian.

## Highlights

- **8 interlinked pages** sharing one stylesheet, one navigation structure and one footer
- **Subscription calculator** with 11 plans, a 1 to 12 month duration slider, an automatic 10% discount from 6 months and an itemized price breakdown
- **Coaches directory** rendered dynamically from `coaches.json` via the Fetch API, with an embedded fallback dataset and image error handling so the page never renders empty
- **Contact form validation** using regular expressions, with real-time feedback on blur and input, inline error messages and Romanian-diacritic-aware name matching
- **Login / logout flow** built as a dynamically injected modal, with session state kept in `localStorage` and `sessionStorage`
- **Interactive touches:** a Canvas 2D orbit animation, rotating hero messages with fade transitions and randomized HSL hover colors on service links

## Pages

| Page | File | Description |
|---|---|---|
| Home | `gym.html` | Hero section with rotating motivational messages, canvas animation, embedded video, brand introduction |
| Services | `servicii.html` | Overview cards for Fitness, Yoga and Cardio |
| Fitness / Yoga / Cardio | `fitness.html`, `yoga.html`, `cardio.html` | Service detail pages with photo galleries and embedded video |
| About | `about.html` | Brand story and the coaches section loaded via AJAX |
| Pricing | `tarife.html` | Interactive subscription calculator plus Silver, Gold and Diamond plan breakdown |
| Contact | `contact.html` | Validated contact form |

## Technical Overview

### Subscription calculator (`calculator.js`)

Computes price from a plan lookup table and a duration slider, recalculating on every change. Subscriptions of 6 months or longer receive a 10% discount, shown as a separate line in the breakdown.

```
total    = monthly price x months
discount = floor(total x 0.10)   if months >= 6, otherwise 0
final    = total - discount
```

### Coaches loader (`coaches-loader.js`)

Fetches `coaches.json`, builds one card per coach through DOM manipulation (name, specialization, experience, certifications list, bio, photo) and appends them to the grid. If the request fails or returns empty data, an embedded copy of the dataset is rendered instead. A broken image falls back to a default photo through an `onerror` handler.

### Form validation (`contact-validation.js`)

Each field has its own rules: minimum length, a required flag and a regex where relevant.

- Name: letters and spaces only, including `ăâîșț`, 2 to 50 characters
- Email: pattern-checked format
- Title: minimum 3 characters
- Description: minimum 10 characters

Fields validate on blur, then re-validate on every keystroke once they are in an error state. On a valid submit the message is stored in `localStorage` and a confirmation is displayed.

### Authentication demo (`auth.js`)

Injects a Login / Logout item into the navigation bar on every page, opens a modal login form, checks credentials against `users.json` and persists the current user in `localStorage`. If `users.json` cannot be loaded, for example when opening the files directly from disk, it falls back to a built-in demo account so the flow still works.

### Other scripts

| File | Purpose |
|---|---|
| `motivational.js` | Cycles ten hero messages every 3 seconds with a fade transition and changes the text color every 5 seconds |
| `canvas-animation.js` | Draws an animated orbiting circle on a 2D canvas using `requestAnimationFrame` |
| `random-color-hover.js` | Applies a random HSL color to service links on `mouseenter` and resets it on `mouseleave` |

## Project Structure

```
proiect-web/
├── gym.html                 # Home
├── servicii.html            # Services overview
├── fitness.html
├── yoga.html
├── cardio.html
├── about.html               # About + coaches section
├── tarife.html              # Pricing + calculator
├── contact.html             # Contact form
├── gym.css                  # Shared stylesheet
├── auth.js
├── calculator.js
├── canvas-animation.js
├── coaches-loader.js
├── contact-validation.js
├── motivational.js
├── random-color-hover.js
├── coaches.json             # Coach data
├── users.json               # Demo accounts
└── poze/                    # Images
```

## Running Locally

No installation is required.

```bash
git clone https://github.com/alexandramariab/Responsive-Fitness-Website.git
cd Responsive-Fitness-Website/proiect-web
```

Serve the folder with any static server so that `fetch()` can load the JSON files:

```bash
python -m http.server 8000
```

Then open `http://localhost:8000/gym.html`.

Opening `gym.html` directly from disk also works, because the coaches section and the login both have built-in fallbacks. A local server is still recommended since it exercises the real JSON loading path.

**Demo login:** `demo` / `demo123`

## Design Decisions

- **Vanilla JavaScript only.** Every feature is implemented with the platform's own APIs (Fetch, DOM, Canvas, Web Storage), which keeps the site dependency-free and instantly deployable on any static host.
- **Graceful degradation.** Features that depend on external data (coaches, users) ship with fallbacks, so a failed request degrades the experience instead of breaking the page.
- **Data separated from presentation.** Coach profiles and user accounts live in JSON, so content can be changed without touching markup or logic.
- **One shared layout.** All pages reuse the same top bar, navigation with a services dropdown and footer, with body classes (`service-page`, `about-page`) selecting page-specific styling.

## Limitations

This is a front-end project, and some parts are intentionally simplified:

- The login is a client-side demonstration. Credentials are stored in a plain JSON file and checked in the browser, so it is not secure authentication and should not be used as such. A production version would need a backend with hashed passwords and server-side sessions.
- The contact form saves submissions to `localStorage` only. No message is sent anywhere.
- The brand, coaches, prices and contact details are fictional.

## Possible Next Steps

- Add a backend (Node.js or Python) for real authentication and form handling
- Make the calculator plans data-driven from JSON instead of a hardcoded table
- Accessibility pass: ARIA attributes for the modal and dropdown, keyboard navigation and contrast checks
- Add a class timetable page driven by JSON

## Author

**Alexandra-Maria Bratan**
Computer Science student, University of Bucharest
[GitHub](https://github.com/alexandramariab)
