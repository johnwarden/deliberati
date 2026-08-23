# deliberati.io - Sunset

This site has been sunset and redirects all traffic to [jonathanwarden.com](https://jonathanwarden.com).

## Background

Deliberati.io was a 2023 Hugo-based syndication hub for Jonathan Warden's writings. The site is being retired because:

- **Brand consolidation**: "Deliberati" is an LLC entity name, not the primary public brand
- **Content redundancy**: Essays published here are maintained on the canonical site, jonathanwarden.com

The live site continues to redirect visitors to https://jonathanwarden.com until DNS is reconfigured.

## Implementation

This repository now contains a simple static redirect page that works with GitHub Pages custom domains. The redirect uses multiple methods for maximum compatibility:
- HTTP meta refresh
- JavaScript location redirect
- Manual link fallback

## DNS

DNS configuration (Cloudflare/Namecheap) remains unchanged and is managed separately by Jonathan R. Warden or Chief of Staff.
