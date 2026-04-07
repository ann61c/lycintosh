# Classic Macintosh Web Emulator

Copy and paste this Claude Code CLI command to have an agent build a working browser-based classic Macintosh emulator in one pass.

## Claude Code CLI

```bash
mkdir mac-emulator && cd mac-emulator

claude "$(cat <<'PROMPT'
Build a framework-free static site that boots a real classic Macintosh System 6 in the browser.

The core requirement is a working emulator, not a visual approximation. Do not build a Mac-like interface in HTML/CSS/JS and call it done. Do not produce a portfolio shell, a fake Finder, or a simulated desktop rendered outside the emulator canvas. If the majority of effort goes toward frontend decoration rather than making the Macintosh actually boot, the implementation is going in the wrong direction.

Treat this as a single-pass build. Make reasonable implementation decisions independently. Prefer the simplest readable structure. Do not use a frontend framework unless strictly necessary.

Use these references:
- Working visual and runtime reference: https://satyricon.uk/macintosh/
- PCE browser runtime source: https://github.com/jsdf/pce
- Original PCE project: https://www.hampa.ch/pce/
- System 6.0.8 source media reference: https://www.macintoshrepository.org/1778-mac-system-os-6-x-6-0-6-0-1-6-0-2-6-0-3-6-0-4-6-0-5-6-0-6-6-0-7-6-0-8-6-0-8l-

Primary goal:
- The page boots to a working Macintosh Finder in the browser
- The environment must be a real System 6 instance, not a mockup
- The page presents a single fixed-size Macintosh centered on a restrained dark stage
- The result should feel closer to the reference at satyricon.uk than to a portfolio page

Definition of done:
- A real emulator canvas boots to a usable Finder
- Boot assets load correctly and the machine reaches the desktop

Definition of failure:
- A fake Mac-like desktop built with frontend code instead of a real emulator
- A page that looks like a Macintosh but does not run one
- A page where the emulator is secondary and most content lives in a separate web UI
- Stopping at scaffolding without verifying a real boot path

Non-negotiable constraints:
- Use plain HTML, CSS, and JavaScript
- The screen must be a real emulator canvas, not a visual mockup
- The Macintosh must be fixed-size; do not responsively squeeze or crop the bezel
- If the viewport is narrower than the machine, allow horizontal scrolling rather than clipping
- Keep the outer UI minimal: no sidebar, no fake web desktop outside the emulator, no extra furniture
- Include only subtle physical framing around the emulator — a drive slot and a small power LED are sufficient
- Show a brief status label while boot assets load; dismiss it once the machine is running

Boot asset rules:
- The Mac Plus ROM is a user-supplied local file; do not fetch it, commit it, or replace it with a placeholder
- Expect or produce a boot asset set equivalent to:
  - A browser PCE runtime JavaScript bundle
  - A PCE Mac Plus WebAssembly binary
  - A PCE Mac Plus extension ROM
  - A user-supplied Mac Plus ROM
  - A PCE config file
  - A bootable hd1.qed disk image
- Keep boot assets, ROMs, disk images, and all generated private outputs out of version control
- Structure the repository so the code can be public while the bootable version still works locally on a machine with the private assets
- Document clearly which local assets are required and where to place them

Implementation guidance:
- Inspect the working reference and PCE source before making assumptions about the runtime shape
- Prioritize emulator wiring and the boot path before visual polish
- If a bootable hd1.qed already exists locally, use it; otherwise build one from the System 6.0.8 floppy images using local HFS-aware tooling or qemu-img
- Do not stop at scaffolding — verify the local version actually boots in the browser
- If the only blocker is the missing user-supplied ROM, leave the repository otherwise complete and explain that blocker clearly
- Do not fabricate ROMs or boot media

Deployment goal:
- Make the project suitable for static deployment via Wrangler and Cloudflare Workers Static Assets
- The repository must remain public-safe while still allowing a developer with the local boot assets to deploy a fully functional version
PROMPT
)"
```

## References

- Working visual/runtime reference: [satyricon.uk/macintosh](https://satyricon.uk/macintosh/)
- PCE emulator project: [jsdf/pce](https://github.com/jsdf/pce)
- Original PCE project site: [hampa.ch/pce](https://www.hampa.ch/pce/)
- System 6.0.8 image source reference: [Macintosh Repository](https://www.macintoshrepository.org/1778-mac-system-os-6-x-6-0-6-0-1-6-0-2-6-0-3-6-0-4-6-0-5-6-0-6-6-0-7-6-0-8-6-0-8l-)
