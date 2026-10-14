# Build summary: Manifesto Teleprompter

## Status
Built and tested locally. Published from this repo (`ENTP-3000`, branch `main`, folder root) with GitHub Pages.

## Live URL
https://jwhitbeck44.github.io/ENTP-3000/

`qr.png` encodes this URL.

## Files
- `index.html`: the whole app. Inline CSS and JS, no libraries, no outside requests, no em dashes, no mention of AI.
- `qr.png`: 1800 x 1800 black-on-white QR code for the URL above, with a quiet zone.
- `BUILD-SUMMARY.md`: this file.

## What was built
- Setup screen: header, the tagline, a script box, a word count with a read-aloud estimate at 140 words per minute, timer mode choice, Start button. The script is kept in sessionStorage so a reload does not lose it; it is cleared when the tab closes.
- Teleprompter: large text scrolling up. Slider (5 to 100) and up/down arrow keys (steps of 2) set speed. Tapping the text area or pressing space pauses and resumes. Scroll speed scales with font size, so the same setting feels similar on a phone and a laptop. Scrolling stops when the last line reaches the reading position; the timer keeps running.
- Timer counts up. "60-second rehearsal": white, red from 1:00. "Full manifesto": white under 3:00, green from 3:00 through 5:00, red from 5:01. A text note under the timer repeats the state so color is not the only signal.
- Record button: requests camera and mic, shows a mirrored preview at the top center, records. Pressing Record restarts the script and timer from the top so the video and timer match. Stop saves `manifesto-YYYY-MM-DD.mp4` (or `.webm`) and shows a link to save it again if the download does not start. Format is chosen by what the browser supports, MP4 first.
- If the camera or mic is blocked or missing, one sentence says so and the teleprompter keeps working.
- Screen wake lock is requested where the browser supports it, so phones do not dim during a take.
- Nothing is sent anywhere. The test run recorded zero network requests.

## What was tested (Chromium via Playwright, fake camera, simulated clock)
- 375px wide: no horizontal scroll on the setup screen or the teleprompter screen. Start button was below the fold at 375x740 in the first run; the script box height was reduced afterward (30vh). That final change was not re-screenshotted.
- Scrolling moves, pauses on tap, resumes on space, stays still while paused.
- Arrow keys: up raised speed 25 to 29 in two presses, down lowered it to 27.
- Rehearsal timer: white at 0:59, red at 1:02.
- Full timer: white at 1:39 and 2:59, green at 3:00, 4:58 and 5:00, red at 5:01 and 5:05.
- Scroll stops at the end of a short script (checked with a two-line script).
- Recording with Chromium's fake camera: preview shown, REC badge, timer reset to 0, Stop produced `manifesto-2026-10-14.mp4`. ffprobe read it as a 3.95 second file with VP9 video and Opus audio.
- Camera denied (getUserMedia rejected): the one-sentence message appeared and the timer kept running.
- No page errors and no external requests in the console and network log.

## Not tested or open
- Not tested on a real phone, Safari, or Firefox. iOS Safari download behavior for blob links is the most likely trouble spot; the "save again" link is the fallback.
- Chromium wrote VP9 video inside an MP4-named file. Most players open it; some older editors may not. Safari and recent Chrome vary in what they record.
- The fake camera shows a test pattern, so picture quality and lens position were not judged.
- Pause stops the scroll and timer but does not pause the recording.
- Browsers may show a permission prompt the first time Record is used; students must allow it.
- `kill-list-generator` has no BUILD-SUMMARY.md. It is a public repo with a single `index.html` at the root and is live on GitHub Pages, so the same layout was used here.
