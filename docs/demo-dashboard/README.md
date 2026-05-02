# Static Demo Dashboard

Open `docs/demo-dashboard/index.html` directly in a browser.

This is a backend-free snapshot of Hermes Volta's dashboard for hackathon judges. It uses only:

- `index.html`
- `data.json`
- relative files under `assets/`

It does not call `dashboard/api.py`, Telegram, Firecrawl, local memory, `.env`, or any private runtime configuration.

## Included Designs

- RC_LOWPASS 1.6 kHz from hand drawn schematic
- RC_HIGHPASS 500 Hz
- RLC_BANDPASS example
- RLC_NOTCH example
- ECG anti aliasing 100 Hz

Each design includes:

- Bode plot
- Transient validation
- PCB visual
- Filter effect VIN vs VOUT
- Cutoff report
- Artifact links

## Note

The live dashboard remains in `dashboard/`. This static snapshot is only for browsing saved demo history without running the backend.
