# Golf Coach – User Manual
## Schmied Research And Development Pty Ltd
**Motto:** Seeking only peace and friendship, to teach if we are called upon, to be taught if we are fortunate.  
**Version:** 2026.10.11 – Ball Tracing + WhatsApp Optimized Share + Robust Drop

### 1. Introduction
Golf Coach is a private, offline-first Progressive Web App (PWA). It analyses golf swings from driver to putter, Face-On and Down-The-Line, directly in your browser. All pose detection and ball tracing runs on-device. Video never leaves your device. First run downloads ~10 MB MediaPipe model from CDN, then works offline.

### 2. Installation
- **iPhone (Safari):** Open Golf-Coach-PWA.html → Share → Add to Home Screen. Hammer-anvil icon appears.
- **Android (Chrome):** Menu → Install app / Add to Home Screen.
- **Desktop:** Chrome/Edge address bar Install icon.
- **Offline / Blocked CDN:** Use golf-coach-local.html via `python3 -m http.server 8000` then http://localhost:8000/golf-coach-local.html. Same file, loads model via local server.

### 3. Interface Overview
- **Top bar:** Logo (hammer above anvil in black circle, alt="hammer above an anvil inside a black circle"), Company name, motto italic, top-actions: Auto/Desktop/Phone selector (fmt), Install, Light/Dark.
- **Hero:** Title, subtitle private by design, pills.
- **Modes (tabs):** Analyse Swing, Full Bag & Gapping, Live Range + Voice.
- **Helpbar:** Buttons – Manual, Help, Install, GitHub, Drills, Achievements, Export Video, Side-by-Side, Metronome, Original MP4, Skeleton WEBM/MP4, All Videos, WhatsApp Optimized (Annotated 720p) and WhatsApp Original.
- **Drop zone:** `<div class="drop" id="drop" role="button" tabindex="0" aria-label="Drop a golf swing video or click to choose">` with SVG upload icon, bold "Drop a golf swing here", muted "or click to choose – MP4 H.264 works best". Visually hidden file input `position:absolute;width:1px;opacity:0`. Keyboard Enter/Space opens picker. Dragenter/dragover adds class `over` (accent border). Handles DataTransfer.files and items.getAsFile().
- **Controls row2:** Shot selector (Driver, Wood, Hybrid, Long Iron, Mid Iron, Short Iron, Wedge, etc – 14 options), View (Face-On / Down-The-Line), Hand (Right/Left), Level, Speed Fast/Precise (lite 12 FPS vs full), Ghost mode (off/pro/ideal), Tracer toggle, Sticks toggle, Pressure Mat toggle, Ball Trace checkbox, Ball Color Auto/White/Yellow, Sensitivity 1-10, Clear Ball, Run Ball Trace.
- **Views:** Two stages – Left Video+Ghost+Tracer+Ball trail, Right Skeleton+Pressure Mat. Chips bottom-left, cap muted, scrubber with phase markers `tl`, time, phaseJump select.
- **Report area:** Coach's report cards, ball stats card, progress charts (shoulder, tempo, X-factor), gapping chart, share/export card, print.

### 4. How to Film for Best Results
- Film from side at chest height, 3-5 m away, whole body head to toes + club + ball.
- Landscape, 30 fps or more, good light, steady, H.264 Most Compatible on iPhone (Settings > Camera > Formats).
- Face-On: camera perpendicular to target line. Down-The-Line: behind hands, along target line.

### 5. Workflow – Analyse a Swing
1. Click drop zone or drag MP4/MOV/WebM.
2. Status: Loading video → Loading pose model (lite/full) → Analysing % with ETA.
3. Select Shot – sets ideal ballPos, stance, shoulder, hip, X-factor, knee, spine, tempo, weight.
4. Scrub video, phase chips: address, takeaway, backswing, top, downswing, impact, follow, finish. Click chip to jump.
5. Toggles: Ghost (pro ideal white dashed race), Tracer (yellow club path fading line), Sticks (green target line + magenta ball position line), Pressure Mat (weight dot lead/trail), Ball Trace (white fading trail with glow dots, current ball white outer black stroke yellow center).
6. Ball Trace runs automatically after impact if checked. If missed: click overlay video to manually add point. Adjust Ball Color (Auto/White/Yellow) and Sensitivity. Clear Ball resets, Run Ball Trace re-runs detection.
7. Read Ball Stats: detections, flight time, carry est (dx*300 yards), apex %, launch angle. Estimation for relative comparison.
8. Read Coach's report: score /10, findings with severity 0 good 1 warning 2 fault 3 critical, why, tips, drill, ranges.

### 6. Ball Tracing – Technical
- estimateBallStartPos from ankles 27,28 + ballPos offset.
- isWhiteBallPixel: white avg>190 max-min<40, yellow r>170 g>170 b<130.
- findWhiteBlobs: binary mask of white that moved diffThresh 35-sens*2 vs prevData, flood fill 8-neighbour, filter area 3-120, w/h ≤28, aspect 0.35-2.8, compactness <6.
- runBallDetection: seek video from impact to end, drawImage to ballCanvas 640x360 willReadFrequently, ROI predicted +- search window, exclude near body shoulders/elbows/wrists <0.08, score brightness-distance, update velocity vx*=0.995 drag, vy+=0.0015 gravity, predicted gap filling.
- drawBallTrail: lineWidth W/280, fading alpha, glow.
- Manual marking, updateBallStats.

### 7. WhatsApp Optimized Share – NEW
**Buttons:**
- 📤 Share Optimized – WhatsApp (Annotated 720p) – id btnShareWhatsAppGolf
- 📤 WhatsApp Optimized (Original 720p) – second button in shareCard

**What it does:**
- Target 720p max (720x1280), bitrate 1.8 Mbps annotated, 1.8 Mbps original (vs original 8-10 Mbps).
- Calculates target size keeping aspect, even dimensions.
- outCanvas 720p, canvas.captureStream(30) + audio tracks from v.captureStream().
- Mime tries `video/mp4;codecs=avc1.42E01E` (WhatsApp native), then `mp4;codecs=avc1`, `mp4;codecs=h264`, fallback `webm;codecs=vp9/vp8`.
- Draw loop: video frame + skeleton via draw(tmp, idx, false) + brand bar #0f2e1a with Schmied R&D – Golf Coach.
- Auto-trim: address-0.5s to finish+0.5s using phases.
- MediaRecorder chunks → Blob → File → navigator.share({files}) if canShare, else download + opens https://wa.me/?text= with summary `*Golf Coach – driver – Pro 8.2/10* – Annotated 720p optimized...`.
- File name `golf-driver-annotated-172860...mp4`, size ~3-8 MB for 10s clip, perfect for WhatsApp 64 MB limit, Instagram, TikTok.
- Privacy: stays on device, URL.revokeObjectURL after 6s.

### 8. Other Exports
- Original MP4: downloads originalFileRef as-is.
- Skeleton WEBM/MP4: canvas.captureStream of skeleton only.
- All Videos: original + skeleton webm + mp4 + card png.
- Coaching card PNG 1280px wide with 5 frames, metrics, logo strip, motto footer.
- PDF report with screen grabs annotated + skeleton.

### 9. Full Bag & Gapping, Live Range, Drills, Achievements
- Full Bag: save sessions per club, chart carry.
- Live Range: Start camera (HTTPS), voice selector, metronome 🥁, auto-capture.
- Drills library 20+ with when/how 3x10.
- Achievements gold badges, Side-by-Side compare before/after.

### 10. Privacy & Security
- All pose + ball detection in browser, model from storage.googleapis.com + jsdelivr wasm, no trackers, no upload, blob URLs revoked.

### 11. Troubleshooting
- Drop not working: ensure MP4 H.264, click zone, check console, try local server, visually hidden input must be 1px opacity 0 not display:none.
- HEVC error: iPhone HEVC – convert HandBrake Fast 1080p30 or set Most Compatible, Safari Mac plays HEVC.
- Model not loaded: online, not blocking cdn.jsdelivr.net, try local server.
- Ball not detected: increase Sensitivity to 10, contrast background not clothing, ball visible after impact, try White/Yellow mode, manually click.
- Share not working: iOS needs Add to Home Screen for file share, else fallback download + WhatsApp link. Check MediaRecorder mp4 support – Chrome 120+ supports mp4.

### 12. Tips
- Film with ball visible contrast.
- Use tracer for club path, sticks for alignment.
- Use WhatsApp Optimized for quick coach feedback, size <10 MB.
