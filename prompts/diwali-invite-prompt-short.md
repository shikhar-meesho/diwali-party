# Diwali card night invite: build prompt (short)

Paste everything below the line into a new Claude chat.

---

Build `index.html` (plus `og.jpg`): a mobile-first Diwali card-party invite to share on WhatsApp. One file, inline CSS and JS, no frameworks or animation libraries, Google Fonts only. Test on 360px first.

## Details (one `PARTY` config object)

- Host Shikhar. Saturday, 31 Oct 2026, 8 pm (calendar 8 pm–2 am IST).
- A-41, Sector 61, Noida. Maps: https://maps.app.goo.gl/TDNikLcfYvgETcieA?g_st=ic
- Dress: Indian glam, "Dress like you’ve already won."
- Stakes: "Small change". Poker blinds 10/20, Teen Patti boot 10.
- WhatsApp 918800250296. Optional `guest: ""` for a baked-in name.

## Look

- One palette, no section colour changes. Night `#150D2E`, marigold `#F4A51C`, rani `#E0306E`, ivory `#FFF4DF`, gold `#E9C46A`, muted `#BEB2DA`.
- A warm glow appears once the diya is lit.
- Fraunces 300 for headlines, Bricolage Grotesque for UI, Tiro Devanagari Hindi for "शुभ दीपावली".
- No filler copy. Curly apostrophes. Honour reduced motion.
- Background canvas after lighting: rising embers and a few faint drifting suits.

## Flow (taps move the story forward, not scrolling)

1. **Dark open.** A near-black page with a dim diya silhouette and "Tap anywhere to continue". Scroll stays locked. Any tap lights the flame and the glow, and draws a rangoli of petals, scallops and four suits stroke by stroke. Then the title appears ("Bipin, you’ve been dealt in.") with a "Deal me in ↓" button.
2. **Shuffle and deal.** The headline reads "Poker first. Teen Patti after." Tapping the deck runs: riffle ×2, bridge, a fan that flips face up in a wave, flip back and close, then deal 4 cards (When, Where, Wear, Bring) into a 2×2 grid. Tap to flip each. Once all 4 are seen, auto-scroll after about 1s. Add a "Continue ↓" fallback.
3. **Are you in?** Yes and NO buttons. NO dodges 3 times without ever covering Yes, then rotates 180° to read ON and turns marigold. Yes or ON then grows to the screen centre while the background shrinks, blurs and dims. ON gets an italic "Play" above it. Then scroll on.
4. **See you there.** Suit fireworks, "See you at the table, Bipin.", the date and address, and three buttons: Add to calendar (Google Calendar link), Message Shikhar (wa.me with prefilled text), and Directions.

## Personal links

- Read the name from `?to=`, falling back to `#to=` and then `PARTY.guest`. Use `textContent` only.
- `?host=1`, or 5 taps on the lit diya, opens a dialog: paste names and get a Copy and a WhatsApp link for each.

## Preview card

- Absolute `og:url` and `og:image`, a title, a description, and `summary_large_image`.
- `og.jpg` at 1200×630: the lit rangoli on the left, with "You’ve been / dealt in." and the date on the right.

## Don’t repeat these bugs

- No scroll-snap and no CSS smooth scrolling. Use a JS easeInOutCubic scroll.
- Call `preventDefault` on card `pointerdown`.
- The tap that flips NO must not also accept. Don't use `event.detail`.
- Position "Play" from its real height, with `margin: 0`.
- No scroll-pinned sections.

## Before you hand it over

1. In Playwright at 360, 390 and 1280px with touch, run both the Yes and the NO paths.
2. Check that `scrollY` stays still during card taps, and that there are no errors and no sideways scroll.
3. Help me host it on GitHub Pages or a Render static site, then point the og tags at the final URL.
