# Signal ledger (public commitments)

Once per trading day, after the market close, a list of that day's closed signal activity is written down and a SHA-256 fingerprint of the list is computed. Each day's fingerprint also contains the previous day's fingerprint, so the days form a chain: a day cannot be changed, removed or inserted later without breaking every fingerprint after it.

**What is published here:** only the fingerprint, the previous fingerprint, the number of entries / exits / late additions, and the time it was made (`ledger.json`). The fingerprint is also timestamped with OpenTimestamps (a free service that anchors it in the Bitcoin blockchain); the proofs are in `proofs/`.

**What is not published here:** tickers, strategies, prices, signals, performance. The list behind a fingerprint is kept private; on request the full list for any day can be produced and anyone can recompute its fingerprint and compare it with the one published here.

**What this proves:** that the list existed in exactly that form no later than the timestamp, and that rows added afterwards for earlier days are visible (`late_additions` in the next day's list), so signals cannot be back-dated silently.

**What this does not prove:** that the signals are good. It says nothing about performance and is not investment advice.

Verification: `sha256` of the canonical JSON of a revealed list (keys sorted, no spaces) must equal the published `hash`; each day's `prev_hash` must equal the previous day's `hash`.
