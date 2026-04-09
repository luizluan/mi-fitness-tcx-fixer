# 🏃 Mi Fitness TCX Fixer

> Fix broken TCX files from Mi Band / Mi Fitness and import them into Strava without errors.

**[→ Open the tool](https://luizluan.github.io/mi-fitness-tcx-fixer)**

---

## 🤔 Why does this exist?

If you've ever tried to export a workout from **Mi Band** or the **Mi Fitness app** and import it into **Strava**, you've probably seen this:

```
Error: Invalid timestamp
```

This happens because Mi Fitness exports TCX files with missing required fields:

| Problem | Description |
|---|---|
| Missing `StartTime` | The `<Lap>` element has no `StartTime` attribute |
| No Trackpoints | Strava requires at least one `<Trackpoint>` with a `<Time>` tag |
| Wrong HR format | `<HeartRateBpm>` must wrap a `<Value>` tag, not contain the number directly |
| Empty `Sport` field | An empty `Sport=""` attribute can cause parsing issues |

This tool fixes all of that automatically — no server, no upload, everything runs in your browser.

---

## ✨ Features

- 🔧 Fixes all TCX issues that cause Strava to reject Mi Fitness exports
- 🌍 Auto-detects browser language (English, Portuguese, Spanish)
- 🔒 100% client-side — your data never leaves your device
- ⚡ No install, no dependencies, single HTML file

---

## 🚀 How to use

1. Open the tool in your browser
2. Drag and drop your `.tcx` file (or click to select)
3. Check the detected activity info
4. Click **Download Fixed TCX**
5. Import the downloaded file into Strava 🎉

---

## 📁 What gets fixed

The tool takes the raw Mi Fitness output and rebuilds it into a valid TCX structure:

```xml
<!-- Before (Mi Fitness) -->
<Lap>
  <TotalTimeSeconds>3009</TotalTimeSeconds>
  <Calories>397</Calories>
  <HeartRateBpm>127</HeartRateBpm>  ← wrong format, no trackpoints
</Lap>

<!-- After (fixed) -->
<Lap StartTime="2026-04-06T12:55:43Z">  ← StartTime added
  <TotalTimeSeconds>3009</TotalTimeSeconds>
  <Calories>397</Calories>
  <AverageHeartRateBpm><Value>127</Value></AverageHeartRateBpm>  ← correct format
  <Track>
    <Trackpoint>  ← trackpoints added
      <Time>2026-04-06T12:55:43Z</Time>
      ...
    </Trackpoint>
  </Track>
</Lap>
```

---

## ⚠️ Limitations

Since Mi Fitness doesn't export GPS or detailed per-second data, the fixed file will:

- ✅ Show duration, calories and average heart rate on Strava
- ❌ Have no map or route
- ❌ Have no distance (you can add it manually on Strava after importing)

---

## 🛠️ Self-hosting

It's a single `index.html` file with zero dependencies. Just clone and open:

```bash
git clone https://github.com/your-username/mi-fitness-tcx-fixer.git
cd mi-fitness-tcx-fixer
open index.html
```

Or deploy to GitHub Pages, Netlify, or Vercel in one click.

---

## 📄 License

MIT — do whatever you want with it.

---

<p align="center">Made with frustration and then fixed with code 🩹</p>
