# AR product preview: configure it, then put it on your real floor

A small Three.js starter. Configure a product in the browser, tap "View in your room", and the WebXR session drops the exact configuration onto a real surface through the phone camera. No app to install.

The interesting part is that the configured product and the AR product are the same object. You do not export a separate model for AR. Whatever the customer built on the left is what gets placed on their floor, down to the size and the fabric they picked.

## How it works

- The ottoman is a plain `THREE.Group` built from the current config. The desk preview and the AR session both show that one group.
- AR uses WebXR with a hit-test source. A reticle follows the detected floor, and a tap places the product there.
- `renderer.xr.enabled = true` plus `ARButton` is the whole entry point. The rest is turning the hit test into a position.

Read [`index.html`](index.html), it is one file.

## Device support, honestly

- **Android, Chrome:** full AR, this is what the starter targets.
- **Desktop or anything without WebXR:** you get the 3D preview, the AR button stays disabled. The code handles both, nothing throws.
- **iPhone:** Safari does not do WebXR AR. The real-world iOS path is Quick Look with a USDZ file (often paired with `<model-viewer>`), which is a different mechanism and out of scope for a minimal three.js starter. In a production build I generate a USDZ alongside the glTF and hand iOS the native viewer.

You need HTTPS for WebXR. GitHub Pages serves HTTPS, so the live demo works on a phone as is. Locally, use a tunnel or a self-signed https server, plain `http://localhost` will not start an AR session.

## Run it

```bash
npx serve .
# then open the https URL it prints on your phone, or deploy to GitHub Pages
```

## What this is

An educational starter. Generic ottoman, clean code, MIT. The point is the AR plumbing, not the furniture.

A production configurator also carries the real catalogue, manufacturing limits, the USDZ path for iOS, a quote flow and a back office. That part is custom.

## About

I build custom 3D product configurators for manufacturers, AR preview included where it earns its place.

- https://grodev.pl/konfigurator-produktowy-3d
- https://grodev.pl/en/custom-3d-configurator

Dominik Groński, GroDev, Poznań.

## License

MIT, see [LICENSE](LICENSE).
