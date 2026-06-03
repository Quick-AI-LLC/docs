# Quick Signal TA

**Version:** 3.1.0  
**Production:** https://signal.quickai.build  
**Purpose:** **Technical analysis API** for crypto assets. One paid call returns price context, market statistics, and **ten indicators** with natural-language assessments.

## Indicators included

1. Bollinger Bands  
2. SMA (20 / 50 / 200) + trend  
3. EMA (20 / 50 / 200) + trend  
4. VWMA (20) + volume comparison  
5. RSI + zone and assessment  
6. MACD + histogram trend  
7. ATR + volatility assessment  
8. Stochastic (`percent_k`) + assessment  
9. Fractals (simplified)  
10. Volume  

## Use cases

- Agent trading and research workflows  
- Bazaar-discoverable TA with strict JSON Schema for inputs and outputs  
- JSON body or URL query parameter access  

## Endpoints

| Method | Path | Price (USDC) | Auth |
|--------|------|--------------|------|
| `GET` | `/health` | Free | None |
| `POST` | `/signal` | $0.05 | x402 |
| `GET` | `/signal` | $0.05 | x402 |

## Request

### `POST /signal`

`Content-Type: application/json`

```json
{
  "token": "BTC",
  "timeframe": "1d",
  "bollinger_period": 20,
  "rsi_period": 14
}
```

| Field | Required | Default | Description |
|-------|----------|---------|-------------|
| `token` | Yes | — | Symbol, name, or contract address |
| `timeframe` | No | `1d` | `5m`, `15m`, `1h`, `4h`, `1d`, `1w` |
| `bollinger_period` | No | `20` | Integer 5–100 |
| `rsi_period` | No | `14` | Integer 2–50 |

### `GET /signal`

Query parameters: same fields as the POST body. **`token` is required**; missing token returns **400** after payment.

Example:

```text
GET /signal?token=BTC&timeframe=1d
```

## Response (`SignalResponse`)

| Field | Type | Description |
|-------|------|-------------|
| `token` | string | Requested token |
| `resolved_name` | string | Resolved asset name |
| `chain` | string \| null | Chain when known |
| `timestamp` | string | Response time (ISO) |
| `price` | number | Latest close |
| `market_cap` | number \| null | When available |
| `volume_24h` | number \| null | When available |
| `change_24h_pct` | number \| null | When available |
| `timeframe` | string | Candle interval used |
| `indicators` | object | All ten indicator blocks |
| `data_source` | string | e.g. `coingecko` |
| `notes` | string[] | Warnings or fallback notes |
| `cached` | boolean | Served from Redis cache |
| `cached_at` | string \| null | Cache timestamp |

On insufficient OHLCV data, the service may return an error object with `detail`, `partial_data`, and `notes`.

### Stochastic field name

The stochastic output uses **`percent_k`** (not `%K`) for JSON Schema compatibility.

## Architecture

- **Data:** CoinGecko → CoinMarketCap → LiveCoinWatch OHLCV fallback chain  
- **Indicators:** Pure TypeScript (no external TA library)  
- **Cache:** Optional Redis, TTL ~45 seconds  
- **Rate limit:** Per-token queue ~5 seconds  
- **x402:** CDP facilitator; Bazaar POST and GET routes with full per-indicator output schema  
- **Startup:** `validateSettings()` and `validateBazaarAtStartup()` before the server listens  

## Environment variables

### Required

| Variable | Description |
|----------|-------------|
| `CDP_API_KEY_ID` | CDP API key |
| `CDP_API_KEY_SECRET` | CDP API secret |
| `PAY_TO_ADDRESS` | Base settlement wallet |

### Optional

| Variable | Description |
|----------|-------------|
| `COINGECKO_API_KEY` | Higher CoinGecko rate limits |
| `CMC_API_KEY` | CoinMarketCap fallback |
| `LIVE_COIN_WATCH_API_KEY` | Live Coin Watch fallback |
| `REDIS_URL` | Response cache (e.g. `redis://redis:6379/0`) |
| `PORT` | HTTP port (default `8000`) |

## Deploy

```bash
cd quick-signal-ta
cp .env.example .env
docker compose up -d --build
```

Caddy terminates TLS for `signal.quickai.build` per `Caddyfile`.

## Examples

### Health

```bash
curl -s https://signal.quickai.build/health
```

### Paid (awal)

```bash
npx awal x402 pay https://signal.quickai.build/signal \
  -X POST \
  -d '{"token":"BTC","timeframe":"1d"}'
```