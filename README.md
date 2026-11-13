# clone-lin-crvusd-health

Experimental **LIN** clone of Curve crvUSD `Controller._health` — liquidation
health from LLAMMA `get_x_down`, liquidation discount, and the `_full` oracle extra.

**Proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open)** — this repo is the LIN copy only.

- **Upstream (hash-pinned, not vendored):** [curvefi/curve-stablecoin](https://github.com/curvefi/curve-stablecoin) `81b3bea490e9b45221c82e78387920ccfeb05b33`
  - `curve_stablecoin/controller.vy` sha256 `42d82a3c36712cc1397f170d8e6924f7985035be2e54cfcfee1ce3d303a47c6b` blob `ac5e86ccaefffde86ced8ee4fa4b61cc5242206f`
  - LICENSE is informational-only (Swiss Stake AG; no reproduction right). This repository does **not** vendor the Vyper source.
- **LIN kernel:** [`src/lin_crvusd_health.lin`](src/lin_crvusd_health.lin) (sha256 `632b010ec66d0a30d57adaae56c959272bff614b91bd136bc32427c3155832c8`, identical to lin-open)
- **C11 oracle copy:** [`test/oracles/crvusd_health_c11.c`](test/oracles/crvusd_health_c11.c)
- **Proofs (lin-open only):** `examples/crvusd_health/` and `make crvusd-health-proof`
- **Class:** EXPERIMENTAL uint64-scale, `SCALE = 1e9`. Not Ethereum uint256. Not 1e18 SWAD. Not a production controller replacement.

Named goldens (equal `x_down=debt=1e9`): disc 0 → **0** (liquidation line); disc 6% → **-60000000**; 2× collateral → **1000000000**; `_full` extra with 10% price-above-band → **100000000**; zero collateral → **-1000000000**; `debt=0` fail-closed **0**. Naive i64 `4e9*4e9` wraps to **-2446744073709551616**; 128-bit muldiv fail-closes to **0**.

## Reproduce proofs (in lin-open)

```bash
make crvusd-health-proof
# or: python3 test/prove_crvusd_health_external.py
```

## Not claimed

Uint256 / SWAD=1e18 / token-precision mainnet parity.
LIN replacing crvUSD, LLAMMA, Curve, or the EVM. TVL or dollar savings. zk soundness.
LLAMMA `get_x_down` band math (see clone-lin-llamma-xy). This clone is Controller health given `x_down`.
