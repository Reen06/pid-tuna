# AETHR personal PIDTuna integration

This fork retains upstream PIDTuna's analysis code and AGPL-3.0-only license.
It is a client-side log analyzer, not a real-time flight controller. Analysis
results need independent validation before changing physical flight hardware.

Build with `pnpm install --frozen-lockfile`, `pnpm typecheck`, then
`VITE_AETHR=1 VITE_BASE_PATH=./ pnpm build`. The authenticated deployment disables
the offline service worker so it cannot cache an authenticated shell after logout.
Serve the exact corresponding source archive as `dist/source.tar.gz`; the UI has
a Source link. Preserve the upstream license and attribution.

Package with Dockerfile.aethr after configuring OWNER_USERNAME in the staged
nginx configuration. Register as a per-user web app, root proxy mode, isolated
network, trusted SSO headers `X-Aethr-User` and `X-Aethr-Role`, and no mounts. The app server rejects
other identities. Publish no host/LAN HTTP port directly: use AETHR's existing
authenticated proxy. Do not grant Front Door access.

Logs are loaded into browser memory, not uploaded to the server or saved to NAS.
Upstream GPS maps request CARTO/OpenStreetMap tiles: tile coordinates disclose
the viewed area to that provider. Other analysis works without those map tiles.

Rollback: stop/remove only the PIDTuna app registration and container. Keep the
source checkout; existing AETHR apps and NAS data are unaffected. No automated
upstream updates: review and validate changes before a milestone rebuild.
