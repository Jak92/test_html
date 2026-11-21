# GMod Recon v0.2.1 — Android live server map

Standalone Android app with real GMod server snapshots. **No PC/backend or Steam key required**, and **no synthetic demo servers**.

**Sources**
- GSM public REST API, `/api/gmod/servers`: server names, IP/ports, players, maps, last checked. Client loads up to 100 servers per page, ranking by online players. Actual coverage depends on GSM. Timestamps represent snapshots, not real-time packets.
- GeoJS IP geolocation: country, city, estimated latitude/longitude, provider name, and uncertainty radius. These are not actual physical server/home addresses.

**Features**
- MapLibre zoom, geolocation pin clustering, cluster totals for servers or players.
- Active-only, minimum player count, Top 100 in visible map area, search, gamemode when present, watchlist.
- Adjustable coverage: top 100/500/1000/2500 or up to 10,000 listed servers. Saves real snapshots and IP geocache locally. If API fails, shows errors and cached records rather than pretending demo records are live.
- Direct browser-to-GSM and GeoJS HTTPS requests from Android WebView. Third-party CORS/network access is required. Internet also needed for MapLibre and tiles.

**Limits**
- First launch requires internet and public sources to be reachable. Source outages, incomplete discovery, rate limiting or Android WebView CORS can prevent results. This version may require a future first-party data service for full reliability.
- IP pin coordinates are GeoIP estimates; can't reveal a residential house.
- Unknown gamemodes are shown as Unspecified, never guessed from names.
- Standalone debug APK signed by GitHub Actions. CI debug signing key may change between runs: if installing over v0.1.1 fails, uninstall the old test build first (watchlist data may be lost).
- No on-device testing yet.

**Build**
`cd gmod-recon/android && gradle :app:assembleDebug` (Android SDK 35, Java 17, Gradle 8.10.2).
Build via .github/workflows/gmod-recon-apk.yml on the v0.2 live branch. Build artifact contains APK, source ZIP and feed probe.

## New: On-device diagnostics

Tap the 🐞 button on the right side of the map. It opens a diagnostic report and copies it to the Android clipboard. Paste into ChatGPT. The report includes counts, API request failures, loading times, last scan status, cache sizes, and map status. It excludes server IPs and names as well as private information. The report uses an Android WebView JavaScript clipboard bridge; it does not transmit the report to a remote service.
