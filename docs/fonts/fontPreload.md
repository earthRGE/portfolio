# Font Preloading

`display: "swap"` controls what the browser shows while your custom font is still loading. It maps to the CSS `font-display` property.

**What "swap" does**

The browser immediately renders your text in a fallback font (like the system sans-serif), then swaps to your custom font as soon as it finishes loading. Visitors can read the page right away, and the font changes a moment later.

**The alternatives**

- **`block`**: hides the text (invisible) for a short period while the font loads, then shows it. This avoids a visible font change but leaves a blank gap if loading is slow. This is called FOIT (flash of invisible text).
- **`swap`**: shows fallback text immediately, then swaps. This is called FOUT (flash of unstyled text).
- **`fallback`**: a compromise. A very short invisible period, then the fallback, and the custom font swaps in only if it loads quickly. If it's slow, the fallback stays for that page view.
- **`optional`**: like `fallback`, but the browser may decide not to use the custom font at all on slow connections. Good for performance, but some visitors won't see your font on their first visit.
- **`auto`**: lets the browser pick, usually behaving like `block`.

**Why swap is the common choice**

It prioritizes readability. Content is never hidden, which is better for slow connections, accessibility, and perceived speed. The trade-off is **layout shift**: if the fallback and custom fonts have different sizes, text may jump slightly when the swap happens.

**Next.js helps with this**

`next/font` automatically adjusts the fallback font's metrics to match your custom font, which reduces that jump a lot. It also preloads and self-hosts the font, so the swap is often barely noticeable.
