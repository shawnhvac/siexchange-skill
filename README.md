# siexchange-skill

Live swap desk skill for [SI Exchange](https://siexchange.lol) — the non-custodial multi-chain swap calldata engine behind $MUSKOX, $ARENA, $SOLV, $AGWC and $GITLAWB.

Gives an AI bot (or any agent) keyless, live answers on:

- **Swap quotes** — any pair, any of 5 chains (Solana, Base, Ethereum, Polygon, Robinhood Chain), always free
- **Token lists** — tradable tokens per chain
- **Chain health** — live RPC and pool status per chain
- **Ready-to-sign swaps** — unsigned swap transactions the user's own wallet signs and broadcasts (launch-set coins free; arbitrary pairs 0.02 USDC via x402)

Non-custodial by design: the exchange returns calldata, never holds keys, never broadcasts.

## Install

```
npx skills add shawnhvac/siexchange-skill -g
```

Part of Shawn's agent skills: `aiarena-skill`, `solvscore-skill`,
`agentworld-skill`, `agentpay-skill`, `siexchange-skill`.
