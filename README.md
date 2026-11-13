# Shoot Data Calculator

Static, single-file web app. Estimates camera-original storage, dailies and buffer for a shoot, shows the extraction resolution for a delivery aspect ratio, and exports a framing chart per camera.

## Host on GitHub Pages
1. Create a repository and add `index.html`, `cameras.js` and this README.
2. Settings > Pages > Source: Deploy from a branch > `main` / root.
3. Open `https://<user>.github.io/<repo>/`.

No build step, no server. The camera dataset lives in `cameras.js`.

## Add or update a camera
Open `cameras.js`, copy any line, edit it and commit. One line is one camera + sensor mode + codec:

`{"brand":"ARRI","model":"ALEXA 35","sensor":"Super 35","mode":"4.6K 3:2 Open Gate","w":4608,"h":3164,"codec":"ARRIRAW (uncompressed)","mbps24":4548.9,"conf":"H","fixed":0,"maxFps":75},`

- `mbps24`: video bit rate in Mb/s at 24 fps.
- `conf`: H published, M derived, L estimate.
- `fixed`: 1 when the rate is a published maximum that does not scale with fps.

You can also add a camera in the app ("Add a camera that is not in the list"). That saves it in your browser only; use "Copy my cameras as cameras.js lines" and paste the result into `cameras.js` to make it permanent.

## Maths
- Rate at fps = Mb/s at 24 fps x fps / 24 (DJI drone rows are published maximums and do not scale).
- GB per day = Mb/s / 8 x 3600 x hours rolled x number of cameras / 1000.
- Dailies = Apple ProRes target rate (422: 147 Mb/s, 422 HQ: 220 Mb/s at 1080p29.97) scaled by pixels and fps.
- Total = (camera original + dailies) x shoot days x (1 + buffer) x copies.
- Extraction = largest centred rectangle of the delivery ratio inside the recorded frame, after lens desqueeze, rounded to even pixels.

Rates are estimates. Confidence H = manufacturer-published, M = derived, L = estimate. Verify with a test recording before budgeting.
