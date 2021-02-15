# Deposit Rate Subsidization

Anchor Protocol's deposit rate stability is supported by borrow demand from borrower ANC distribution and direct subsidization. Anchor defines a target deposit rate \($$r_{target}$$\) and a threshold deposit rate \($$r_{threshold}$$\) and constantly attempts to retain a deposit rate close to $$r_{target}$$ and always above $$r_{threshold}$$.

Every epoch, the average deposit rate during the last epoch \($$r_{current}$$\) is calculated and compared with the target and threshold rates. Appropriate measures are then made to readjust the deposit rate.

## Borrower ANC Incentives

Anchor's deposit rate is primarily adjusted by calibrating the rate of ANC emission to borrowers \($$e$$\), updated through a feedback control algorithm.

### ANC Emission Feedback Control

Anchor alters the ANC emission rate based on a **multiplicative increase / multiplicative decrease feedback control** algorithm, which adjusts the ANC emission rate of the next epoch $$e_{n+1}$$ based on the previous emission rate of $$e_n$$:

$$
e_{n+1} = k \cdot e_n
$$

* If observed deposit rate is below the target \($$r_{current} < r_{target}$$\), double the emission rate \($$k = 2$$\)
* If observed deposit rate exceeds the target \($$r_{current} > r_{target}$$\), reduce emission to 90% \($$k = 0.9$$\)

## Direct Subsidization

As an additional layer of safety, the protocol directly subsidizes the deposit rate if it is below the threshold rate \($$r_{current}<r_{threshold}$$\), funded from the interest buffer's stockpiled stablecoins. 

An amount required to raise the deposit rate to the threshold is distributed to depositors, which is limited to 5% of the interest buffer's balance per subsidization to prevent excessive drainage. Distributed subsidies are added to the money market’s liquidity, increasing the aToken exchange rate and appreciating the value of aTokens.

