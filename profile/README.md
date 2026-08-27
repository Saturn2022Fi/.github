<div align="center">

<img src="https://saturn2022.com/brand/banner-16x9.png" width="100%" alt="Saturn2022" />

# Saturn2022

**Options on real stocks, priced by no one.**

[saturn2022.com](https://saturn2022.com)

</div>

Every options market runs on someone quoting prices. This one has nobody.
Spot comes from Chainlink, volatility from that same feed's own update
cadence, the premium from Black-Scholes computed on chain at the moment
you buy. Seventeen markets live on Robinhood Chain, including the only
tradable SpaceX call anywhere.

## The idea, in one paragraph

Options are priced on volatility, and a chain has never had it. So every
onchain options venue imports the number: a server signs a quote, a market
maker prices around it. But a Chainlink feed only publishes when the price
has moved a fixed step, so its own update times carry the volatility. Count
the timestamps, and the number the chain could never see falls out of
records it already keeps. The estimator is Cho and Frees (1988) applied
to deviation-triggered oracles. The rest is Black-Scholes in fixed point.

## Design decisions

- **Fully collateralized.** Writing a call escrows the whole share, so there is
  no margin, no liquidation, and no thin pool an attacker can lean on.
- **One oracle read that moves money.** Settlement is the only feed read;
  pushing it means pushing Chainlink itself.
- **Pooled shares.** Small holders write together through a vault, and the
  premiums split by what each put in.
- **No pennies.** The vault refuses any premium below ten basis points of
  spot, because backtesting found every losing write looked exactly like that.

## The code

- **[saturn2022](https://github.com/Saturn2022Fi/saturn2022)** - the option house, its vaults, and 62 passing tests. What the site is built on.
- **[saturn2022-crank](https://github.com/Saturn2022Fi/saturn2022-crank)** - the vault's hands. Writes covered calls against free stock, settles what expired.
- **[saturn2022-tools](https://github.com/Saturn2022Fi/saturn2022-tools)** - measurement scripts. Every number on the site comes from one of these.
- **[saturn2022-paper](https://github.com/Saturn2022Fi/saturn2022-paper)** - the working paper: deviation-threshold oracles are volatility instruments.

## Live

House: [`0xea09f07D7F6FBc61E83e342aB586Ed2147f2d63d`](https://robinhoodchain.blockscout.com/address/0xea09f07D7F6FBc61E83e342aB586Ed2147f2d63d)
Lens: [`0x87A7593659E08b02098d4c3D8F3c236D0414dA81`](https://robinhoodchain.blockscout.com/address/0x87A7593659E08b02098d4c3D8F3c236D0414dA81)
