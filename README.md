# When is the cheapest AI model not the best value? An illustrative cost, value and deal-guardrail model

**Under placeholder assumptions, the cheapest model on API cost fails the quality bar on every example task, and a higher tier is cheaper once failed outputs are priced in. On the seller side, a discount guardrail built on a conservative margin sends 42% of simulated deals, carrying about 91% of contract value, to Finance review.**

> **This is an illustrative exercise.** Prices are public list prices. Token counts, volumes, rework costs, success rates, gross margins and all deals are placeholder or simulated assumptions. It is not company data and not real deal experience.

## Business question

Buyers and sellers of AI model APIs face two linked questions:

1. **Buyer side:** which model gives the best value for a task once quality is included, not just the lowest price per token?
2. **Seller side:** how deep a discount can a deal take before gross margin falls below a floor, and who should approve it?

## Key findings

1. **Discount mechanisms are similar across Anthropic, OpenAI and Google.** Batch processing is 50% off at all three. Cached input reads cost 2.5-10% of the standard input price, depending on provider and model. The differences are in cache write fees, storage fees and the premium charged for speed.
2. **Some headline prices are promotional.** Google's Gemini 4 Argon and Gemini 3.8 Flash list prices are scheduled to double (from 1 December 2026 and 1 January 2027). A deal would need price protection or a committed rate.
3. **Per-token price is not per-task cost.** Cost differs by task because of token volume, how much input can be cached, and whether batch pricing is allowed. Complexity is represented through token volume and success rate, not modelled directly.
4. **Cheapest is not best value (illustrative).** With placeholder success rates and rework costs, the cheapest model on API cost misses the required success rate on all three tasks. The best-value model is a higher tier. This follows from the assumptions: rework cost is far larger than API cost, so quality dominates.
5. **Break-even rework cost shows when a premium tier pays off.** It equals the extra API cost divided by the success-rate gain. If real rework cost is above that figure, the higher tier is cheaper overall.
6. **The maximum safe discount depends on margin.** With a 25% margin floor, the maximum discount is 1 - (1 - margin) / (1 - floor). That is about 47% at 60% gross margin and 20% at 40%.
7. **A margin-aware guardrail concentrates review where the money is.** On 12 simulated deals ($17.8M total contract value), five breach the margin floor under the 40% scenario. They are 42% of deals but about 91% of contract value.

## Recommendation (seller side)

| Approver | Maximum discount |
|---|---|
| Sales rep | 15% |
| Sales manager | 35% |
| Finance | 45% |
| Above 45% | Executive exception |

Add one control: any discount above 20% needs Finance to verify the deal's actual margin. Full reasoning, trade-offs and monitoring metrics are in the memo: `memo/deal_approval_guardrail_memo.pdf`.

## Models and data

| Source | Used for |
|---|---|
| Anthropic API pricing page | Claude Haiku 5.5, Sonnet 5.5, Fable 5.1 |
| OpenAI API pricing page | gpt-6-luna, gpt-6.1-sol, gpt-6-astra |
| Google Gemini API pricing page | Gemini 3.5 Flash-Lite, 3.8 Flash, 4 Argon |

Prices were read from each provider's official page on 20 October 2026 and will go stale. Record the date whenever you update them.

| Sheet | Contents |
|---|---|
| `Summary` | Cheapest model vs best-value model per task |
| `Seller_Guardrail` | Margin scenarios, discount limits, 12 simulated deals, approval load |
| `Breakeven` | Rework cost at which a higher tier pays for itself |
| `Prices` | List prices, post-promo switch, cache and batch parameters |
| `Tasks` | Three example tasks: tokens, volume, rework cost, required success rate |
| `Quality` | Success rate per model and task (placeholders) |
| `Calc` | Cost per task under list, cached, batch and batch plus cached scenarios |

## Method and assumption decisions

- **Tiers, not versions:** one small, one mid and one flagship model per provider. Anthropic's Opus 5.5 is not modelled.
- **Neutral placeholders:** success rates are identical for every provider within a tier, so no provider is favoured. Differences between providers show only after real results replace them.
- **Value formula:** expected cost per task = API cost + (1 - success rate) x rework cost. Models below the required success rate are excluded from best value.
- **Cost scenarios:** list, cached (with a cache hit rate on the cacheable share of input), batch, and batch plus cached. Batch and caching are treated as stacking, per provider documentation. Cache misses pay the cache write multiplier.
- **Simplifications:** Google's hourly cache storage fees are ignored. Prompts are assumed to be under 100k tokens. Tokenizer and reasoning-token multipliers default to 1.0 and should be measured.
- **Margin scenarios:** 40%, 60% and 80% gross margin are placeholders. No provider publishes per-model or per-deal margins, and public estimates vary widely with the definition used.
- **Colour code:** blue = input, black = formula, green = cross-sheet link, yellow = key assumption to replace.

## Limitations

- All success rates, token counts, volumes, rework costs and deals are assumptions or simulations.
- List prices are not negotiated prices.
- Quality is reduced to pass or fail. Latency, rate limits, support terms and data residency premiums are not modelled.
- The guardrail is a deal-level gross margin control. It says nothing about any company's overall profit, which also depends on training and infrastructure costs.
- The model tests discount depth only, not term length, prepayment or commit structure.
- Results are directional and depend on the rework cost assumption, which is the most influential input.

## How to reproduce

1. Open `llm_cost_value_model.xlsx`.
2. Check `Prices` and set the post-promo switch (cell B3) to see price-change risk.
3. Edit the three tasks in `Tasks` to match a real workload.
4. Replace the yellow success rates in `Quality` with pilot results: 30-50 real samples per task per model, graded against a written pass or fail rubric.
5. Choose a margin scenario in `Seller_Guardrail` (cell B4) and read `Summary`, `Breakeven` and the approval table.

## Next steps

- Run a small pilot to replace one column of placeholder success rates with measured ones.
- Measure real token counts with each provider's token-counting tool.
- Add term length, prepayment and committed-spend structures to the seller-side model.

## Author

TH 
