# IEMECON 2027

Conference website for IEEE IEMECON 2027, hosted by the University of Engineering & Management (UEM), Jaipur, India. The site provides conference information for researchers, authors, speakers and attendees, including paper submission guidelines, registration details, committees, the technical program and venue information.

The website is a multi-page static site served through Firebase Hosting. There is no application framework, server-side runtime, database or build step in this repository.

## Technology stack

| Technology | Purpose |
| --- | --- |
| HTML5 | Page structure, navigation and conference content |
| CSS3 | Responsive layouts, shared styling, CSS custom properties and animations |
| Vanilla JavaScript (ES6+) | Countdown timer, navigation, sliders, carousels and scroll interactions |
| [Anime.js](https://animejs.com/) 3.2.1 | Menu transitions and entrance animations |
| [Font Awesome](https://fontawesome.com/) 6.4.0 | Interface icons |
| [Google Fonts](https://fonts.google.com/) | Inter and Outfit typefaces |
| [Firebase Hosting](https://firebase.google.com/docs/hosting) | Static website hosting and preview channels |
| [GitHub Actions](https://github.com/features/actions) | Automated preview and production deployments |
| [Dependabot](https://docs.github.com/en/code-security/dependabot) | Weekly GitHub Actions dependency updates |

Anime.js and Font Awesome load from CDNJS, while fonts load from Google Fonts. Some page backgrounds also use remote Unsplash images. Internet access is needed to load these external resources. Paper submissions are handled by the external [Microsoft CMT portal](https://cmt3.research.microsoft.com/IEMECON2027), not by a backend in this repository.

## Project structure

```text
IEMECON_2027/
├── index.html                     # Conference homepage
├── 404.html                       # Firebase Hosting error page
├── assets/
│   ├── css/
│   │   └── style.css              # Shared styles and responsive layouts
│   ├── js/
│   │   └── main.js                # Shared client-side interactions
│   ├── images/                    # Logos, portraits, banners and venue photos
│   ├── committee/                 # Committee members and session chairs
│   ├── program/                   # Schedule, keynotes and special sessions
│   ├── venue/                     # Jaipur, accommodation and travel information
│   ├── Previous_Conferences/       # Historical conference pages
│   ├── about_us.html
│   ├── gallery.html
│   ├── Contributors.html
│   ├── sister_conference.html
│   ├── call_for_papers.html
│   ├── technical_tracks.html
│   ├── important_dates.html
│   ├── author_guidelines.html
│   ├── submission.html
│   ├── proceedings.html
│   ├── registration_fees.html
│   └── contact.html
├── .github/
│   ├── workflows/
│   │   ├── deploy-preview.yml      # Pull request preview deployment
│   │   └── deploy-prod.yml         # Live deployment on push to main
│   └── dependabot.yml             # GitHub Actions dependency updates
├── .editorconfig                  # Editor formatting conventions
├── .gitattributes                 # Git attributes
├── .gitignore
├── .firebaserc                    # Default Firebase project
├── firebase.json                  # Firebase Hosting configuration
└── README.md
```

## Local development

### Prerequisites

- Git
- A modern web browser
- Python 3 (for the example local HTTP server below), or another static HTTP server

Node.js, npm and a frontend build tool are not required to run the website locally.

### Getting started

Clone the repository and start a local HTTP server from its root:

```bash
git clone https://github.com/iemaserver/IEMECON_2027.git
cd IEMECON_2027
python3 -m http.server 8000
```

Open [http://localhost:8000](http://localhost:8000) in your browser. Edit the HTML, CSS or JavaScript files and refresh the browser to see the changes. Stop the server with `Ctrl+C`.

## Maintaining the website

| Content or functionality | Where to make changes |
| --- | --- |
| Homepage, announcements, speakers and featured dates | [`index.html`](index.html) |
| Conference deadlines | [`assets/important_dates.html`](assets/important_dates.html) and the homepage |
| Countdown target | [`assets/js/main.js`](assets/js/main.js) |
| Visual styling, colors and responsive behavior | [`assets/css/style.css`](assets/css/style.css) and page-specific inline styles |
| Menus, hero slider, carousels and scroll-to-top controls | [`assets/js/main.js`](assets/js/main.js); some homepage animations and carousel logic are inline in `index.html` |
| Author instructions and submissions | [`assets/call_for_papers.html`](assets/call_for_papers.html), [`assets/author_guidelines.html`](assets/author_guidelines.html) and [`assets/submission.html`](assets/submission.html) |
| Fees and registration instructions | [`assets/registration_fees.html`](assets/registration_fees.html) |
| Committees and program details | [`assets/committee/`](assets/committee/) and [`assets/program/`](assets/program/) |
| Images and branding | [`assets/images/`](assets/images/) |

Each page is a separate HTML document. Navigation and footer markup are repeated across pages, so changes to shared links or branding should be applied consistently across the affected files. Use relative asset paths appropriate to each page's directory depth and preserve filename capitalization.

The homepage includes version query parameters on the shared CSS and JavaScript URLs. Review these parameters across affected pages when releasing changes to cached assets.

## Hosting and deployment

The site uses Firebase Hosting. [`firebase.json`](firebase.json) sets the hosting public directory to the repository root (`.`), and [`.firebaserc`](.firebaserc) specifies `iemecon-2027` as the default Firebase project. No build or compilation is necessary before deployment.

### Automated deployments

| Workflow | Trigger | Destination |
| --- | --- | --- |
| [`deploy-preview.yml`](.github/workflows/deploy-preview.yml) | Pull request | Firebase Hosting preview channel, expiring after 30 days |
| [`deploy-prod.yml`](.github/workflows/deploy-prod.yml) | Push to `main` | Firebase Hosting live channel |

Both workflows use `FirebaseExtended/action-hosting-deploy@v0` and require the following GitHub Actions secrets:

- `FIREBASE_SERVICE_ACCOUNT` — Firebase service account credentials with the required deployment permissions.
- `FIREBASE_PROJECT_ID` — target Firebase project ID.
- `GITHUB_TOKEN` — automatically provided by GitHub Actions.

Keep service account credentials out of the repository.

### Firebase CLI (optional)

For local Hosting emulation, install the Firebase CLI using npm (requires Node.js and npm), then run:

```bash
npm install -g firebase-tools
firebase emulators:start --only hosting
```

Open the Hosting URL shown by the CLI. For an authorized manual production deployment, use:

```bash
firebase login
firebase deploy --only hosting --project iemecon-2027
```

Confirm the target project before deploying. Prefer the GitHub Actions workflow for routine releases.

**Hosting scope:** Because the repository root is the public directory, non-hidden files not covered by the hosting ignore rules may also be served. Do not add sensitive files to the repository; review the hosting ignore list when adding development-only files.

## Contributing

1. Create a branch from `main` for your changes.
2. Follow the formatting conventions in `.editorconfig`.
3. Preview the site locally and check desktop and mobile layouts.
4. Verify navigation, relative asset paths, browser console errors, interactive elements and any changed conference information.
5. Open a pull request and review the Firebase preview deployment before merging.

There is currently no automated test suite or lint/build command in the repository. Use the manual checks above when reviewing changes.
