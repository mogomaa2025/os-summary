# Project Design

## Purpose

OS Summary is a static study portal for Operating Systems and Computer Networks. Students can browse lecture notes, review summaries and mindmaps, and practice multiple-choice questions in English and Arabic.

## Architecture

The site uses standalone HTML documents with embedded CSS and vanilla JavaScript. A static server delivers the files, and the browser handles navigation, animations, question rendering, and scoring. There is no required backend, database, authentication service, package manager, or build pipeline.

GitHub Pages hosts the published site. Relative links support hosting under the `/os-summary/` project path.

## Content and Navigation

| Location | Responsibility |
| --- | --- |
| `index.html` | Landing portal with links to the two subject hubs |
| `os.html` | Operating Systems lecture hub |
| `nw.html` | Computer Networks hub |
| `oslec/` | OS lessons, coverage, summaries, and general quizzes |
| `oslec/osmcq/` | Topic-specific OS quizzes and a final exam |
| `nwlec/` | Networking, security, distributed systems, summaries, and quizzes |
| `nwlec/nwmcq/` | Topic-specific networking quizzes and a final exam |
| `quiz-ans.html` | Supporting quiz answers |
| `cppstrings.html` | Additional C++ strings study material |

The main journey is portal → subject hub → lesson or quiz → subject hub. Documents can also be visited directly. Hub listings are maintained in JavaScript filename arrays; adding a file alone does not automatically add it to navigation. The OS hub includes an alternative directory-scan API helper, but its normal workflow uses the embedded list and requires no API server.

## Visual Design

The portal and subject hubs use a terminal-inspired appearance: dark gradients, monospace typography, bright accent borders, glowing text, and animated decorative backgrounds. The landing page uses green accents; the OS hub uses blue accents. Cards and prominent navigation controls provide entry points into the study material.

Lesson and quiz pages have their own embedded styles rather than a shared stylesheet. Some topic quizzes load Bootstrap 5.3.0 from `cdn.jsdelivr.net` and Font Awesome 6.4.0 from `cdnjs.cloudflare.com`. These resources require browser Internet access for their intended styling and icons.

Arabic pages use right-to-left document direction, with English technical terms mixed into the content. Changes should preserve readable text direction, mobile layouts, and clear answer feedback. Keyboard-accessible answer controls, visible focus states, and reduced-motion support are recommended improvements to assess page by page, rather than assumed existing features.

## Quiz Behavior

Question data lives inside individual HTML files. JavaScript renders choices and updates feedback in the browser. Behavior varies by page: the networking overview quiz evaluates a selected answer immediately, marks the correct choice, reveals an explanation, and updates the score; other quizzes use a submission action.

Answer keys are delivered to the browser with the page. This design suits self-study, but does not provide secure exam grading. There is no central service for saving results or synchronizing progress between devices.

## Development and Validation

Use the existing checkout. No dependency installation or compilation is required. From the repository root, run:

```sh
python3 -m http.server 8000 --bind 127.0.0.1
```

Validate that the portal, both subject hubs, a lesson, and representative quizzes return successful HTTP responses. In a browser, follow subject navigation, answer correct and incorrect quiz choices, check explanations and scores, and inspect Arabic layout and narrow screens. HTTP checks alone do not verify interactive behavior or visual layout.

When adding study content, keep filenames and relative links consistent, update the relevant hub's explicit list, and check the resulting destination. There is no repository-provided automated test suite or build configuration in the current checkout.

## Maintenance Constraints

Standalone pages keep deployment simple and allow individual lessons to be shared directly. Their duplicated CSS, scripts, and question markup make consistency a manual responsibility. A shared stylesheet or quiz engine could reduce duplication in a future coding task; neither is required to run the current site.

Keep credentials and private data out of HTML and JavaScript: all published files and embedded question data are public to visitors. Preserve compatibility with static hosting when extending navigation or introducing new dependencies.
