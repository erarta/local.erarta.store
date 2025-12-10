# Cloudflare Tunnel Setup for local.erarta.store

## Prerequisites

- Docker installed on M2 Pro MacBook
- Cloudflare account with erarta.store domain

## Step 1: Install cloudflared

```bash
brew install cloudflared
```

## Step 2: Login to Cloudflare

```bash
cloudflared tunnel login
```

This opens a browser - select the erarta.store domain.

## Step 3: Create the Tunnel

```bash
cloudflared tunnel create local-erarta
```

This creates a tunnel and outputs a **Tunnel ID** (save it!).

A credentials file is created at:
`~/.cloudflared/<TUNNEL_ID>.json`

## Step 4: Copy Credentials

```bash
# Copy the credentials file to this repo (it's gitignored)
cp ~/.cloudflared/<TUNNEL_ID>.json ./tunnel-credentials.json
```

## Step 5: Update config.yml

Edit `cloudflared/config.yml` and replace `<TUNNEL_ID>` with your actual tunnel ID.

## Step 6: Create DNS Record

```bash
cloudflared tunnel route dns local-erarta local.erarta.store
```

This automatically creates a CNAME record pointing to your tunnel.

## Step 7: Run Everything

```bash
docker compose up -d
```

## Verify

- n8n: https://local.erarta.store
- SSH: Via separate tunnel (port 22)

## Useful Commands

```bash
# Check tunnel status
cloudflared tunnel list

# View tunnel info
cloudflared tunnel info local-erarta

# Delete tunnel (if needed)
cloudflared tunnel delete local-erarta
```
