# RTSP View

A static, mobile-friendly camera viewer built for GitHub Pages. It uses **HLS** (hls.js / native Safari) or embeds the **MediaMTX WebRTC player**.

> A web browser cannot directly play `rtsp://` URLs. GitHub Pages is static hosting: it cannot proxy or convert RTSP.

## Start from your existing RTSP camera

Install [MediaMTX](https://github.com/bluenviron/mediamtx) on a machine that can reach your RTSP feed. The repository includes `mediamtx.yml`. Set the environment variable `MTX_PATHS_CAMERA_SOURCE` to the real RTSP URL, e.g.:

**Linux/macOS:**
```sh
MTX_PATHS_CAMERA_SOURCE='rtsp://192.168.1.100:8000/camera' ./mediamtx mediamtx.yml
```

**PowerShell:**
```powershell
$env:MTX_PATHS_CAMERA_SOURCE = 'rtsp://192.168.1.100:8000/camera'
.\mediamtx.exe mediamtx.yml
```

Your gateway then serves (by default):
- HLS: `http://GATEWAY_IP:8888/camera/index.m3u8`
- WebRTC player: `http://GATEWAY_IP:8889/camera`

These local URLs are for **local testing**. GitHub Pages uses HTTPS; the viewer requires a reachable HTTPS gateway URL, with valid TLS and appropriate CORS settings for HLS (and WebRTC transport properly configured). Set up an HTTPS reverse proxy or an equivalent secure endpoint. A private LAN address generally cannot be reached by remote visitors. Do not forward RTSP ports or expose private camera credentials on a public website.

## Publish to GitHub Pages

Repository: https://github.com/ViniciusDeMartinViude/RtspView

1. Open **Settings → Pages** on GitHub.
2. Under **Build and deployment**, choose **Deploy from a branch**.
3. Select **main**, directory **/(root)**, and **Save**.
4. After GitHub deploys the site, open https://viniciusdemartinviude.github.io/RtspView/ .

Select HLS or MediaMTX WebRTC page and paste the **HTTPS gateway playback URL**. The field does not accept `rtsp://` directly.

## Security and compatibility

- No camera URL or password is persisted by this site; the browser accesses your selected gateway directly.
- A MediaMTX WebRTC iframe must allow embedding and have ICE/network configuration suited to your network.
- An HLS gateway must supply CORS headers allowing the GitHub Pages origin and a browser-compatible H.264/AAC stream.
- If access is meant to be private, add authentication and access restrictions at the gateway; an open GitHub Pages viewer does not secure the video.
