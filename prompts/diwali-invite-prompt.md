# Diwali card night invite: build prompt

Paste everything below the line into a new Claude chat.

---

Build me a single-file, mobile-first HTML web invite for a Diwali card party. It will be shared as a link on WhatsApp, so it must look and work perfectly on phones (360–430px wide) first, then desktop. One self-contained `index.html`: inline CSS and JS, no frameworks, no animation libraries (use CSS transitions, vanilla JS and canvas). Only external resource: Google Fonts.

## Party details

Keep these in one editable `PARTY` config object at the top of the script.

- Host: Shikhar
- Date: Saturday, 31 Oct 2026, 8 pm till the last hand (calendar: 8 pm–2 am IST)
- Venue: A-41, Sector 61, Noida
- Maps: https://maps.app.goo.gl/TDNikLcfYvgETcieA?g_st=ic
- Dress code: Indian glam. "Dress like you’ve already won."
- Stakes: Poker blinds 10/20, Teen Patti boot 10 (card label: "Small change")
- Host WhatsApp: 918800250296
- guest: "" (optional baked-in guest name for a one-off copy)

## Concept

A quirky-but-luxe invite (50/50 funny and festive). Poker first, Teen Patti after. Inspired by a viral reel where an invite has a rigged YES/NO wheel and "NO" is just "ON" upside down. The experience is a short story driven by taps, not long scrolling. Each step is one full-height screen, and the page moves forward on interaction.

## Design language

- One constant palette across the whole page. Never switch section backgrounds (no green felt section, no colour jumps). The background stays night indigo throughout; the only change is a warm glow fading in once the diya is lit.
- Tokens: night `#150D2E`, night-2 `#22144A`, marigold `#F4A51C`, rani pink `#E0306E`, ivory `#FFF4DF`, gold `#E9C46A`, muted `#BEB2DA`, card ink `#2A1A3F`.
- Type: Fraunces (light 300, high optical size) for headlines; Bricolage Grotesque for UI and body; Tiro Devanagari Hindi for "शुभ दीपावली". Sentence case, no all-caps labels, no eyebrow labels.
- Use typographic apostrophes (’) in all visible text.
- No commentary or filler copy on the page. Only headlines, card details, button labels, and one instruction on the first screen.
- Ambient layer (fixed canvas behind content, starts after the diya is lit): about 28 slow rising marigold embers plus about 7 large faint card suits (♠♥♦♣, ~13% opacity) drifting upward and slowly rotating.
- Respect `prefers-reduced-motion` (skip animations, show end states). Visible focus rings. No horizontal scroll at any width.
- `viewport-fit=cover` with safe-area padding.

## Screen 1: dark open

- Starts almost black (`#0B0718`). Scroll is locked until the first tap.
- Centre: a diya (SVG: clay bowl `#B5531C` with gold rim and three gold dots, teardrop flame with a radial gradient ivory → marigold → rani), shown as a dim silhouette (`filter: brightness(.3) saturate(.15)`) with the flame hidden.
- Bottom: "Tap anywhere to continue" in muted text, gently breathing opacity.
- Tap anywhere (`pointerup` on document, plus Enter/Space):
  - The diya brightens to full colour.
  - The flame pops in with a springy scale, then flickers forever.
  - A warm radial glow fades in behind.
  - A rangoli around the diya draws itself stroke by stroke (SVG paths with `pathLength=1` and `stroke-dashoffset` 1 → 0, staggered delays): inner gold circle, 8 marigold petals, marigold circle, 16 rani scallops, outer rani circle, then the four suits fade in at the compass points.
- About 1.7s later, reveal "शुभ दीपावली", the headline "You’ve been dealt in." (or "Bipin, you’ve been dealt in." when personalised), and a small "Deal me in ↓" button that scrolls to screen 2.

## Screen 2: shuffle and deal

- Headline: "Poker first. Teen Patti after." Use a non-breaking space inside "Teen Patti" and `text-wrap: balance`.
- A stacked deck of about 11 cards (rani back with a gold diagonal lattice and ivory border; ivory faces with rank and suit, red suits in rani) and a "Shuffle and deal" button.
- Tapping the deck or the button runs one choreographed sequence (CSS transform transitions, async/await sleeps):
  1. Riffle twice: halves split left/right with a slight tilt, then interleave back with a stagger.
  2. Bridge: the deck lifts and scales up slightly, then settles with a springy ease.
  3. Fan: cards spread into an arc (about −38° to +38°, transform-origin well below the cards) and flip face up in a wave (3D `rotateY` with `backface-visibility`).
  4. Hold, flip back face down in reverse order, and sweep the fan closed.
  5. Deal: four detail cards fly out of the deck one by one (FLIP: start at the deck centre, scaled down and randomly rotated, then animate to their grid spots). The deck fades.
- Four detail cards sit in a 2×2 grid (4 across on desktop), face down showing a suit. Tapping flips each one: When / Where / Wear / Bring, each with a label, value and small detail line.
- Once all four have been flipped at least once, wait about 1.1s and auto-scroll to screen 3.
- Show a "Continue ↓" button after the deal as a fallback.

## Screen 3: are you in?

- Headline: "So, are you in?" or "So Bipin, are you in?"
- Two pill buttons: Yes (filled marigold) and NO (outlined marigold).
- NO dodges on touch `pointerdown` (with `preventDefault`), mouse `pointerenter`, or keyboard Enter/Space. It jumps to a random spot inside the arena and never overlaps Yes.
- After 3 dodges, the 4th attempt flips it: it returns to its place, rotates 180° so "NO" reads "ON", and turns filled marigold like Yes.
- Takeover (same move for Yes and ON):
  - The chosen button grows from its exact position to a large pill centred on screen (FLIP using position and size transitions with a springy ease) and pulses a halo twice.
  - Everything behind it scales to about 0.92, blurs (8px) and dims.
  - For ON, an italic gold "Play" fades in above it, so it reads "Play ON".
  - Then clear the blur, fade the takeover, and scroll to screen 4.

## Screen 4: see you there

Hidden until the guest accepts.

- Card-suit fireworks on canvas: 5 staggered bursts of ♠♥♦♣ in palette colours, with gravity and fade.
- Headline: "See you at the table." or "See you at the table, Bipin."
- Recap: "Saturday, 31 Oct, 8 pm" and the address.
- Stacked buttons:
  - Add to calendar: a Google Calendar template link with title, dates, address, and details including stakes, dress code and the maps link.
  - Message Shikhar: `wa.me/918800250296` with the prefilled text "Bipin here. I’m in for Diwali card night 🪔♠️".
  - Directions: the maps link.
- Footer: "Hosted by Shikhar".

## Personalisation

- Read the guest name from `?to=Name` (fallback: `#to=Name`, then `PARTY.guest`).
- Capitalise it and insert it into the hero, RSVP and finale headlines, the document title, and the WhatsApp message.
- Set it with `textContent`, never `innerHTML`.
- Hidden host tool, opened with `?host=1` or by tapping the lit diya 5 times: a dialog where I paste names (one per line) and get one link per guest, each with Copy and WhatsApp-share buttons. Share message: "Bipin, you’ve been dealt in 🪔♠️ <link>".

## WhatsApp preview

- Tags:
  - `og:title`: "You’ve been dealt in: Diwali card night"
  - `og:description`: "Poker first, Teen Patti after. Saturday 31 Oct, Noida."
  - `og:type`, `og:url`, `og:image` (absolute URL), and the image width and height
  - `twitter:card`: `summary_large_image`
- Generate `og.jpg` at 1200×630:
  - Night background with a warm glow.
  - The lit diya inside the rangoli on the left.
  - On the right: "शुभ दीपावली", "You’ve been / dealt in." in Fraunces, and "Diwali card night, Saturday 31 Oct" in gold italic.

## Bugs to avoid

These all happened in earlier versions.

- No scroll-snap and no CSS `scroll-behavior: smooth`. They made the page jump up and down when cards were tapped. Use a custom JS smooth scroll with easeInOutCubic.
- Call `preventDefault` on `pointerdown` for the flip cards so focus doesn't scroll the page.
- On touch, a tap fires `pointerdown` then `click`. The tap that flips NO to ON must not also count as accepting. Never rely on `event.detail`.
- The dodging NO must never land on top of Yes on 360px screens.
- Keep text clear of buttons: give the "Play" line `margin: 0` and position it from its real height.
- Don't drive the story with long scroll-pinned sections. They rendered badly on phones.

## Testing

Before handing over, test in a headless browser (Playwright) at 360×640, 390×844 and 1280×800 with touch.

1. Tap to light.
2. Shuffle and deal.
3. Flip all four cards, and check that `scrollY` stays still.
4. Check the auto-advance.
5. Run the NO dodge → ON → takeover path.
6. Run the Yes takeover path.

Check for console errors and horizontal overflow, look at screenshots of each step, and fix anything that's off.

## Hosting

Deliver `index.html` and `og.jpg` as a folder, then help me host it as a static site. Either:

- GitHub Pages: create a repo and push via the GitHub API with a short-lived token I give you, then enable Pages.
- Render: a Static Site from the GitHub repo, with publish directory ".".

Once the final URL is known, point `og:url` and `og:image` at it, and give me a test link with `?to=Bipin`.
