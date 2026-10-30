# Nova Proxy 5.0.0

A release about the TLS handshake: the ECH key resolver now defaults to the one combination that works from Iran, raw links can carry a decoy SNI for Nova desktop clients, and the panel tells you how exposed each of your hosts is and what every config format actually sends.

## ECH resolves its key over plain DNS at 1.1.1.1

ECH (Encrypted Client Hello) hides the real server name inside the handshake, but the client first has to fetch Cloudflare's ECH key from DNS. Since October 2026 that lookup works from inside Iran over plain DNS to 1.1.1.1 and fails over the AliDNS DoH resolver the previous releases used. The default is now `udp://1.1.1.1`. Panels still set to AliDNS or to an empty resolver migrate on load, so nothing to do by hand. The ECH card has a resolver preset with the plain DNS option marked as recommended, the public DoH resolvers if you prefer one, and a custom field. The hint on the card carries the date of the measurement, because filtering changes and a resolver that works this month may not next month.

## Decoy SNI for Nova desktop clients

Raw VLESS and Trojan TLS links can now carry a decoy server name and a method. A Nova desktop client that understands them sends the decoy in the first packet and the real name afterwards. The feature is off by default. Turn it on in the new Decoy SNI card, add up to 32 decoy hosts, and pick a method. Decoys that are empty, invalid, or equal to one of your real hosts are dropped at save time and the card says which and why. The parameters are written only into the raw VLESS and Trojan TLS links; Clash, sing-box, Shadowsocks and plaintext configs never see them, and clients that do not know the parameters ignore them.

## The panel shows what each host exposes

Every host in the Domains card gets a risk chip: Custom domain, Shared host (a `workers.dev` or `pages.dev` name), or Often filtered. When a panel has no custom domain at all, one note under the list and a banner on the dashboard say so. Below the Domains card, a new "What each config sends" card groups your config formats by what they put on the wire (address, SNI, host header, ECH, decoy) so you can see at a glance which links depend on a shared host. The card loads from a new cookie-gated admin audit endpoint and has a Retry button if the request fails. Subscription responses also name the endpoint host and its risk class in two response headers, and a host dropped by the health check now falls back to the configured endpoint host instead of whatever hostname the request arrived on.

## Resistance Policy

The policy card gains an Enhanced fragment row (the xray fragment layer) and no longer lists ECH as a warning, because ECH is now on by default for new panels and is the right setting.

## Smaller changes

- The dashboard side column fills down to the policy card and the Recent activity list scrolls instead of growing without end.
- Copy about what reaches Iran is dated instead of stated as a permanent fact.
- The repository's `npm run build` is now a no-op, because the built `worker.js` ships in the repository. The Deploy to Cloudflare button works again.

## Updating

Panels do not update themselves. Use the update button in your panel, or the Update option in the Telegram bot. Your users, settings and data are kept. After updating, your panel should report **5.0.0**.
