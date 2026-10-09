---
name: siexchange
description: Live swap desk for SI Exchange (siexchange.lol), the non-custodial multi-chain swap engine. Use when the user asks for a swap quote, token prices, what $MUSKOX $ARENA $SOLV $AGWC or $GITLAWB are worth, which chains SI Exchange supports, or a ready-to-sign swap transaction on Solana, Base, Ethereum, Polygon or Robinhood Chain. Quotes and launch-set coins are free; no API keys needed.
---

# SI Exchange — live swap desk

Goal: give the user a live swap quote or a ready-to-sign swap transaction
on any supported chain, with the numbers pulled fresh from the exchange.

Success means: the user gets the current quote (amounts, hops, approvals)
or an unsigned transaction they can inspect, with a link to
https://siexchange.lol where they can trade it themselves.

Stop when: the question is answered with a verifiable number and link.

## What this is

- SI Exchange is non-custodial: it returns READY-TO-SIGN swap
  transactions. The user's wallet signs and broadcasts — the exchange
  never holds keys and never broadcasts.
- 5 chains: solana, base, ethereum, polygon, robinhood.
- Launch-set coins ($MUSKOX, $ARENA, $SOLV, $AGWC, $GITLAWB) trade FREE.
  Arbitrary token pairs cost 0.02 USDC/query via x402 on Base L2.
- Quotes are always free, any chain, any pair.

## Commands (free, no key)

**Exchange health — which chains are live right now**
```
curl -s -A "Mozilla/5.0" https://siexchange.lol/health
```

**Tradable token list per chain**
```
curl -s -A "Mozilla/5.0" "https://siexchange.lol/tokens?chain=base"
```

**Live swap quote (any chain, any pair)**
```
curl -s -A "Mozilla/5.0" "https://siexchange.lol/quote?chain=base&tokenIn=USDC&tokenOut=AGWC&amountIn=1000000"
```
Returns amountOut, hop-by-hop pool path, and required approvals.
amountIn is in RAW integer units (USDC = 6 decimals, most EVM tokens 18).

**Ready-to-sign swap transaction (launch-set coins = free)**
```
curl -s -A "Mozilla/5.0" -X POST https://siexchange.lol/calldata \
  -H "Content-Type: application/json" \
  -d '{"chain":"base","tokenIn":"USDC","tokenOut":"AGWC","amountIn":"1000000","taker":"0x<wallet>"}'
```
Returns an UNSIGNED transaction (to, data, value, chainId) plus the
approval the wallet needs first. The user signs — the exchange never
holds keys. taker is required: the checksummed EVM address or Solana
pubkey that will sign. Arbitrary (non launch-set) pairs return HTTP 402
first — that is the x402 payment gate at 0.02 USDC, not an error.

## How to answer

- Always pull live quotes. Pool prices move block to block.
- Lead with the number (what 1M USDC-units of X buys of Y), then the
  route (hops, pools), then the link https://siexchange.lol.
- Never say "the exchange sent the trade" — it never broadcasts. Say the
  user signs and sends from their own wallet.
- A 402 on /calldata is the payment gate for arbitrary pairs. Quote the
  terms it carries (0.02 USDC, Base, facilitator). Launch-set coins skip
  it entirely.
- If a chain shows rpc_ok:false in /health, say that chain is degraded
  right now and check /health again before retrying.
