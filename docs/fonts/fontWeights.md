# Font Weights

`weight: "100 900"` tells the browser the range of weights your variable font file supports. It's a range written as "minimum maximum", not a single value, and it maps to the CSS `font-weight` axis inside the font.

**Why it's needed**

A variable font is one file that can render any weight between its lightest and heaviest. The browser can't see inside the file to know that range, so you declare it. With `"100 900"`, you're saying: "this one file covers every weight from 100 (thin) to 900 (black)."

That makes any `font-weight` in that range work. `300`, `400`, `650`, and `700` all render correctly from the same file, including in-between values like `550` that static fonts can't do.

**What happens with a wrong range**

- **Range too narrow** (say `"400 400"`, but the font goes to 900): asking for bold may not use the real bold. The browser can fake it by smearing the regular weight, which usually looks worse.
- **Range too wide** (say `"100 900"`, but the font only covers 400 to 700): the browser thinks light weights exist, so `font-weight: 300` won't fall back sensibly, and it uses the nearest weight in an unpredictable way.

Neither breaks the site, but matching the real range gives the most accurate rendering.

**How to find the real range**

- Check the font's page on its source website, which lists the available weights.
- Look at the file names in the `static` folder (Light, Regular, Bold, and so on) and convert them to numbers using the table from my last message.
- Or upload the variable file to a tool like Wakamai Fondue (wakamaifondue.com), which shows the exact weight axis range.

**Static vs variable**

For a static font you set one number, like `weight: "700"`, because each file is a single weight. A range with two numbers only makes sense for variable fonts.

Since you said the static folder has bold fonts, your range might be something like `"400 700"`. Check the file names to be sure.

# Font Weights: Size Differences

Sizes are rough ballparks for a typical Latin-only font. Your Agrave Pro files will differ, so check them with:

```bash
ls -lh app/fonts/ static/
```

## Static files (one file per weight)

| Weight | Number | Approx. size (.ttf) | Approx. size (.woff2) |
|---|---|---|---|
| Light | 300 | 60-120 KB | 25-50 KB |
| Regular | 400 | 60-120 KB | 25-50 KB |
| Medium | 500 | 60-120 KB | 25-50 KB |
| SemiBold | 600 | 60-120 KB | 25-50 KB |
| Bold | 700 | 60-130 KB | 25-55 KB |
| Black | 900 | 60-130 KB | 25-55 KB |

Individual weights are close in size. Heavier weights are only slightly larger because their outlines are a bit more complex. Weight alone barely changes file size.

## Variable file (all weights in one)

| File | Approx. size (.ttf) | Approx. size (.woff2) |
|---|---|---|
| Variable (100-900) | 150-400 KB | 60-150 KB |

## Total payload comparison

| Setup | Files loaded | Approx. total (.woff2) |
|---|---|---|
| 1 static weight | 1 | 25-50 KB |
| 3 static weights (400, 500, 700) | 3 | 75-150 KB |
| 6 static weights | 6 | 150-300 KB |
| 1 variable file | 1 | 60-150 KB |

## Takeaways

- **One or two weights:** static files are usually smaller.
- **Three or more weights:** the variable file usually wins, with fewer requests.
- **.woff2 beats .ttf:** about 30-50% smaller than the .ttf files Google Fonts gives you.
- **Hero title only:** if you use a single bold weight, one static Bold file may be the lightest option.
- **Target:** keep total font payload under roughly 100-150 KB for a fast site.
