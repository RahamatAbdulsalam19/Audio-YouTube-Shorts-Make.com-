# Audio → YouTube Shorts (Make.com)

Upload long-form **audio** and get **captioned, on-brand 9:16 YouTube Shorts**—auto-titled, scheduled, logged, and alert-monitored. Built in **Make.com** with Google Drive, Google Sheets, and YouTube Data modules.

> **Why this exists:** creators and teams spend hours clipping, captioning, titling, and uploading. This pipeline turns that into a background task.

---

## What it does

* **Ingest** audio from a watched Drive folder or URL
* **Transcribe** and **score hooks** (by keywords, silence, duration)
* **Slice** the best segments to **1080×1920 (9:16)**
* **Burn captions** (word-by-word), apply brand template, safe-area text
* **Generate titles & hashtags** (based on transcript/keywords or overrides)
* **Upload/Schedule** to **YouTube Shorts**
* **Log** clip URL, topic, length, and status to **Google Sheets**
* **Alert** via email/Slack if any step fails

---

## Stack

* **Make.com** (Scenario + Router + Iterator + Error handler)
* **Google Drive** (watched folder)
* **Google Sheets** (log + manual overrides)
* **YouTube** (upload/schedule)
* Optional: **Slack/Email** for alerts

---

## Quick start

1. **Import the scenario**

   * Make → **Scenarios → Create a new scenario → … (menu) → Import blueprint**
   * Select `blueprint/make-blueprint.json`

2. **Create connections**

   * **Google Drive** → folder that receives input audio
   * **Google Sheets** → log/override sheet
   * **YouTube** → channel where Shorts will be uploaded

3. **Copy the override sheet**

   * Upload `templates/override-sheets-template.csv` to Drive and **Open with Google Sheets**
   * Paste the new Sheet URL into the scenario where required

4. **Test run**

   * Drop an MP3/WAV into the watched folder → **Run once** in Make
   * Confirm: a Short appears in YouTube **(private/unlisted as you prefer)** and a row logs in Sheets

5. **Schedule**

   * In Make, set **Scheduler → Every 15 minutes** (or your cadence)

---

## Google Sheet schema (log + overrides)

| Column           | Type    | Purpose                                |
| ---------------- | ------- | -------------------------------------- |
| `audio_url`      | string  | Source of the file (Drive/URL)         |
| `title_override` | string  | Force a custom title (optional)        |
| `tags`           | string  | Comma-separated hashtags (optional)    |
| `start`          | seconds | Force start time for a clip (optional) |
| `end`            | seconds | Force end time for a clip (optional)   |
| `clip_url`       | string  | Resulting YouTube URL (output)         |
| `status`         | enum    | `uploaded` / `scheduled` / `failed`    |
| `topic`          | string  | Detected topic/keyword (output)        |
| `length_sec`     | number  | Clip duration (output)                 |
| `notes`          | string  | Any additional info/errors             |

> If `start`/`end` are present, the pipeline **uses your timestamps** instead of auto-selected hooks.

---

## Configuration tips

* **Branding/captions:** keep text inside safe areas for Shorts; adjust font/size/placement in the video-assembly step.
* **Length targets:** 20–45s is a sweet spot for Shorts; the selector tries to hit this window.
* **Privacy:** default upload can be **Unlisted**; switch to **Public** after QA or schedule a publish time.
* **Naming:** clip filenames follow a consistent pattern for easy tracking (e.g., `yyyymmdd_title_snippet-01.mp4`).

---

## Error handling

* The scenario writes `status=failed` + `notes` on errors
* Optional alert: add a **Slack/Email** module after the error handler to ping your team
* Common misses:

  * Sheet URL not set or permissions blocked
  * YouTube quota/auth scopes not granted
  * Oversized audio or unsupported format

---

## Roadmap

* Optional **multilingual captions**
* **AB test** title variants and keep the winner
* Write first-48h metrics back to the Sheet for learning loops
* Add **TikTok/Reels** outputs in parallel



## Credits

Built by **Rahamat Abdulsalam** — Data Analyst & AI Automation Expert.
If this saved you time, star the repo and say hi on LinkedIn.

**Start here:** import the blueprint, connect Drive/Sheets/YouTube, drop an audio file, and watch the Short appear.
