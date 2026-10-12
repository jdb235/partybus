# Live bus departures — stop 12539

A single-file bus departures display using HTML, CSS, and vanilla JavaScript. All settings, API requests, display rendering, and automatic refresh behavior are inside **`index.html`**. No external JavaScript libraries are needed.

`dev_server.py` is **only** a static file web server: it does not request, modify, or proxy bus predictions.

## Run on your local network (development)

Install Python 3 if needed. Open a terminal in this folder and run:

```sh
python3 dev_server.py
```

On your development computer, visit `http://localhost:8000`.

On another device on the same LAN, visit `http://YOUR-COMPUTER-IP:8000` (for example, `http://192.168.1.42:8000`).

To find your computer's IP address:

- Linux: `hostname -I`
- macOS (usually Wi-Fi): `ipconfig getifaddr en0`
- Windows: `ipconfig` (look for the active adapter's IPv4 address)

The server listens on `0.0.0.0` by default, so other LAN devices can connect if their network and your computer's firewall allow port 8000. Set a different port if needed: `python3 dev_server.py --port 9000`.

## API requests (in both development and standalone use)

The browser **directly** requests this URL every 15 seconds:

```text
http://10.15.231.250:8080/predictions/12539
```

This happens whether the page is loaded from `http://YOUR-COMPUTER-IP:8000` or directly as a `file://` HTML file. The **display device** itself needs network access to `10.15.231.250:8080`.

**CORS requirement:** The API is a different origin from your dev server and from the standalone file. The API must permit cross-origin browser requests, usually by returning:

```http
Access-Control-Allow-Origin: *
```

Without that response header, the browser can block JavaScript from reading the response **even though the endpoint is reachable**. A Python static web server cannot fix CORS on a separate API. If you're able to modify the API, configure this header there. Browser local-network and `file://` security policies may require extra testing on the final display device.

## Standalone use (no development server)

Copy **only `index.html`** onto the device, then open it in a browser. No Python or development server is needed, as long as the browser permits the request to the API.

## Change settings

Edit the constants near the beginning of the `<script>` section of `index.html`:

- `STOP_ID`: currently `12539`
- `API_URL`: API host/port and predictions path
- `POLL_INTERVAL_MS`: 15000 (every 15 seconds)

The departure board uses the API response's `stop_name`, `stop_id`, `route`, `headsign`, `minutes`, `color`, and `text_color` fields.
