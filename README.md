# 🎁 SOFTSWISS Bonus Toolkit

A Tampermonkey userscript for the SOFTSWISS backoffice that automates bonus eligibility checking, reward calculation, and form autofill for Personal Bonuses and Freespins.


# [!WARNING ]
This script is currently in active testing and should be used with caution.
Results may not always be accurate. Always review the calculated reward before issuing a bonus — do not issue anything solely based on the script's output without a manual sanity check

---

## Features

- **Bonus Calculator** — checks player eligibility and calculates the correct reward with one click
- **Personal Bonus Autofill** — prefills the issue form with the right bonus, currency, wager, expiry, amount, and comment
- **Freespins Autofill** — selects the correct Loyalty FS Bgaming bonus and prefills spin count and comment
- **Crypto support** — fetches live USD rates via CoinGecko for crypto-currency players
- **VIP escalation alert** — flags rewards over 200 USD equivalent for manager approval
- **Blocking tag detection** — stops calculation immediately if the player has restricted tags (e.g. `BA`, `Only VIP manager cashback`)

---

## How It Works

### Eligibility Checks
Before calculating a reward, the script verifies all of the following:

| Check | Requirement |
|---|---|
| Balance | < 1 (or < 1 USD equivalent for crypto) |
| Pending cashouts | Must be 0 |
| Spent in casino | Must be > 0 and shown in green |
| Bonus ratio | Must be < 50% |
| Player tags | Must not include any blocking tags |

### Reward Calculation
1. Finds the most recent **Personal**, **Deposit**, or **Loyalty FS Bgaming** bonus issued to the player
2. Checks for any successful cashout after that bonus — if found, deposits are only counted from after that cashout
3. Sums all successful deposits in that window
4. Calculates the reward (rounded up to the nearest 5):
   - **< 100 USD in deposits** → Free Spins (deposits ÷ 2, rounded up to nearest 5)
   - **≥ 100 USD in deposits** → Cash Bonus (10% of deposits, rounded up to nearest 5)
5. For crypto players, all thresholds and calculations use the live USD equivalent

### Autofill
After running the calculator, opening the Personal Bonus or Freespins popup will automatically prefill:

**Personal Bonus form**
- Bonus: `Personal Bonus`
- Currency: player's active currency
- Wager multiplier: `40`
- Valid until: today + 7 days
- Amount: calculated reward (if bonus type matched)
- Comment: `losses`

**Freespins form**
- Freespin bonus: `{CURRENCY} Loyalty FS Bgaming {CURRENCY}` matching the player's currency
- Spins count: calculated reward (if freespins type matched)
- Comment: `ly`

---


## Usage

1. Open a player profile in the backoffice
2. Click the **🎁 Bonus Calc** button (fixed, top-right corner)
3. The script will:
   - Check eligibility
   - Expand the payments table to 100/page (unless the player has fewer than 10 payments)
   - Calculate the reward
   - Show the result in a popup
4. If eligible, open the **Issue Personal Bonus** or **Issue Freespin Bonus** popup — fields will be prefilled automatically

---



- The button only appears on player profile pages (`/players/{id}`) — not on the dashboard or other pages
- The payments table is only expanded if 10 or more rows are visible (10 is the default page size — fewer than 10 means all payments are already showing)
- The calculator result is stored in memory and used to prefill forms only for the same player — navigating to a different player clears it
- Rewards over 200 USD equivalent trigger an escalation warning: `🔺 ESCALATE TO VIP`
