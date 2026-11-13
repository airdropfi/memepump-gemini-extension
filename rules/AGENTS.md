# MemePump for Gemini CLI

MemePump (https://memepump.org) checks Solana tokens for rug-pull and honeypot signals. This extension connects Gemini CLI to the `memepump` MCP server (`https://memepump.org/api/mcp`). The tools are `check_token_safety`, `get_token_holders` and `explain_risk_signals`. No sign-in is needed.

If the MemePump tools are not available, tell the user to run `/mcp` and check that `memepump` is listed, then restart Gemini CLI.

## Rules that always apply

1. **Read-only.** MemePump tools only read data. They cannot trade, sign or move funds, and no tool takes a wallet address, seed phrase or private key. If the user offers a seed phrase or private key, tell them not to share it.
2. **Valid mint.** `mint` is a base58 Solana token address, 32 to 44 characters. If the user gives a name or ticker instead, ask for the mint address; tickers are copied by fake tokens.
3. **Report as returned.** Give the risk level and score (0 is lowest risk, 100 is a scam), the honeypot signal, liquidity lock (null means unknown), whether the mint and freeze authority are active, holder concentration and the warnings. Name the source as shown in the result (RugCheck analysis of on-chain data) and say when it was checked (`checked_at`; `cached` means a stored result up to 4 hours old).
4. **Not advice.** Say the result is informational and not financial advice, and that a low risk score is not a guarantee. Never tell the user to buy or sell.
5. **Unknown is not safe.** If the tool returns an error or data is unavailable, say so and offer to retry later. Do not guess a verdict.
6. **Limits.** Calls without an API key are limited per IP (HTTP 429 carries `Retry-After`: wait that long once). A 401 means the API key is wrong or inactive: stop and tell the user.
7. **Keys stay out of chat.** An API key is optional. Never ask the user to paste one into the conversation.

## Commands

- `/memepump:check <mint>`: safety report for one token (read-only).
- `/memepump:holders <mint> [count]`: largest holders and wallet clusters (read-only).
- `/memepump:explain [signal]`: what a report signal means and what it cannot show (read-only).

If the user asks for something in these areas without a command, follow the same rules.
