OPTIONAL — exact typefaces
==========================
The app is designed in Newsreader (headings) and Public Sans (interface).
It runs perfectly without them, falling back to the platform serif and sans.

To ship the exact design, download the two families from Google Fonts,
convert to woff2, and drop the files here with these exact names:

  Newsreader.woff2          (weights 400-600, variable or 400+600 static)
  Newsreader-Italic.woff2   (italic 400)
  PublicSans.woff2          (weights 400-700)

Then run:  npx cap sync android

No code change is needed — the @font-face rules in index.html already
point at these paths, and the fallbacks disappear once the files exist.
