# Deposit Rate Subsidization

Anchor Protocol's deposit rate stability is supported by two mechanisms, ANC distribution incentives and direct subsidization. The protocol defines a target deposit rate \($$r_{target}$$\) and a threshold deposit rate \($$r_{threshold}$$\), constantly attempting to retain a deposit rate close to $$r_{target}$$ and always above $$r_{threshold}$$.

Every epoch, the average deposit rate during the last epoch \($$r_{current}$$\) is calculated and compared with the target and threshold rates. Appropriate measures are then made to readjust the deposit rate.

## Borrower ANC Incentives

Anchor's deposit rate is typically adjusted by constantly calibrating the rate of ANC emission to borrowers \(e\). The emission rate is updated via a feedback control mechanism.

### ANC Emission Feedback Control

The feedback control mechanism alters the ANC emission rate based on the degree of deviation between the current deposit rate \($$r_{current}$$\) and the target deposit rate \($$r_{target}$$\). The ANC emission rate of the next epoch, $$e_{n+1}$$ is adjusted from the previous emission rate of $$e_n$$:

$$
e_{n+1} = f\left(\frac{r_{target}}{r_{current}}\right) \cdot e_n
$$

The multiplier function $$f$$ should be a continuous monotonically increasing concave function with a fixed point of $$f(x) = 1$$ , set to maintain a constant emission rate when the current deposit rate matches the target deposit rate. Through this equation, the emission rate increases when the deposit rate is below the target and decreases when the deposit rate exceeds the target.

$$f$$ is initially set as the square root function but may be updated through protocol governance.

$$
e_{n+1}=\sqrt{\frac{r_{target}}{r_{current}}} \cdot e_n
$$

The protocol caps the maximum emission rate to 10 ANC / day \(example value\).

## Direct Subsidization

If the deposit rate is observed to be below the threshold, this is comprehended as 

Interest Buffer Pool

Markets periodically distribute subsidies to depositors whenever the deposit APY is below the threshold rate.

$$
r_{current}<r_{threshold}
$$

Besides holding on to future subsidies, the interest buffers are responsible for calculating the amount of stablecoins required to increase the deposit rate to 10%. To prevent excessive drainage of the interest buffer, only up to 5% of its balance can be used per subsidization process.

Distributed subsidies are added to the money market’s liquidity, increasing the aToken exchange rate. Depositors indirectly receive subsidies via value appreciation of their aTokens.

