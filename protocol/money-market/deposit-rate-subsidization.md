# Deposit Rate Subsidization

Anchor Protocol's deposit rate stability is supported by two mechanisms, ANC distribution incentives and direct subsidization. The protocol defines a target deposit rate \($$r_{target}$$\) and a threshold deposit rate \($$r_{threshold}$$\) and constantly attempts to retain a deposit rate close to $$r_{target}$$ and always above $$r_{threshold}$$.

Every epoch, the average deposit rate during the last epoch \($$r_{current}$$\) is calculated and compared with the target and threshold rates. Appropriate measures are then made to readjust the deposit rate.

## Borrower ANC Incentives

Anchor's deposit rate is typically adjusted by constantly calibrating the rate of ANC emission to borrowers \($$e$$\). The emission rate is updated via a feedback control mechanism.

### ANC Emission Feedback Control

The feedback control mechanism alters the ANC emission rate based on the degree of deviation between the current deposit rate \($$r_{current}$$\) and the target deposit rate \($$r_{target}$$\). Anchor uses a **multiplicative increase / multiplicative decrease feedback control** algorithm, which adjusts the ANC emission rate of the next epoch $$e_{n+1}$$ based on the previous emission rate of $$e_n$$:

$$
e_{n+1} = k \cdot e_n
$$

* If the deposit rate is below the target \($$r_{current} < r_{target}$$\), $$k = 2$$
* If the deposit rate exceeds the target \($$r_{current} > r_{target}$$\), $$k = 0.9$$

## Direct Subsidization

If the deposit rate is observed to be below the threshold, this is comprehended as 

Interest Buffer Pool

Markets periodically distribute subsidies to depositors whenever the deposit APY is below the threshold rate.

$$
r_{current}<r_{threshold}
$$

Besides holding on to future subsidies, the interest buffers are responsible for calculating the amount of stablecoins required to increase the deposit rate to 10%. To prevent excessive drainage of the interest buffer, only up to 5% of its balance can be used per subsidization process.

Distributed subsidies are added to the money market’s liquidity, increasing the aToken exchange rate. Depositors indirectly receive subsidies via value appreciation of their aTokens.

