
**Please be aware that this is initial guidance; you may encounter multiple issues that need to be iterated on many rounds to fix them.**


---
Create a portable Claude skill that turns a long video into captioned viral shorts.

The skill should combine two workflows:

1. Viral shorts clipping: find engaging moments, cut 10+ vertical 9:16 clips by default, center human faces when present, and avoid awkward crops.
2. Subtitle styling: burn in captions using selectable built-in styles, with a default viral style where the currently spoken word is highlighted green and slightly larger.

The skill should ask only necessary clarification questions:

- how many shorts the user wants
- whether subtitles should be enabled
- which subtitle style to use, or whether the user wants to describe a custom style
- whether to require human faces in every clip

Defaults if the user does not specify:

- create at least 10 shorts
- use 9:16 vertical format
- enable subtitles
- use the viral green word-highlight subtitle style
- subtitles should be 2 lines by default
- keep clips around 20-60 seconds
- run as fast as practical with parallel processing

Technical requirements:

- Use FFmpeg/ffprobe for video cutting, scaling, cropping, encoding, and burning subtitles.
- Use Whisper or an available speech-to-text tool to create transcripts with timestamps.
- Support word-level timestamps when available; if not available, estimate word timing from transcript segments.
- Use OpenCV, MediaPipe, or another local face/person detection tool for portrait reframing.
- Prefer face-aware crop for talking-head clips.
- Use face/person detection for reframing. Center the speaker’s face when confidence is high.
- If detection is weak or no human is present, fall back to content-safe cropping or contain mode instead of blindly center-cropping.
- For clips without clear faces, use a safe contain/fit strategy so text, slides, or screen recordings are not cut off.
- Use parallel processing where possible: transcript analysis, clip scoring, face scan, and FFmpeg exports.
- Output a clip plan/report with selected moments, timestamps, scores, and export paths.
- Output clips into a clear folder like `viral-shorts-[source-name]/`.
- Keep dependencies local and portable. Do not hardcode absolute paths.
- If a required tool is missing, explain what is missing. Ask before downloading or installing dependencies.
