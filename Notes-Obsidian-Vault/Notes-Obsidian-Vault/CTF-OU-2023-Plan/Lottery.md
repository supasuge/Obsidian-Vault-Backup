## Table of Contents

    - [SageMath Calculations:](#SageMath\Calculations:)
- [Probabilities of each event](#probabilities\of\each\event)
- [Creating a dictionary of events and their probabilities](#creating\a\dictionary\of\events\and\their\probabilities)
- [Sorting events by probability](#sorting\events\by\probability)
- [Displaying the sorted events](#displaying\the\sorted\events)
- [Lottery-Crypto](#lottery-crypto)
  - [Calculated Probabilities](#Calculated\Probabilities)

To perform the calculations for the order of likelihood of these events in SageMath, we will calculate the probabilities of each event and then compare them. The events are:

  

1. Correctly guessing a random 128-bit AES key on the first try.

2. Winning a lottery with 1 million contestants.

3. Winning a lottery with 1 million contestants 5 times in a row.

4. Winning a lottery with 1 million contestants 6 times in a row.

5. Winning a lottery with 1 million contestants 7 times in a row.

  

### SageMath Calculations:

  

In SageMath, we can calculate the probabilities as follows:

  

```sage

# Probabilities of each event

prob_AES_key = 1 / (2^128)

prob_lottery_1 = 1 / (10^6)

prob_lottery_5 = (1 / (10^6))^5

prob_lottery_6 = (1 / (10^6))^6

prob_lottery_7 = (1 / (10^6))^7

  

# Creating a dictionary of events and their probabilities

events = {

    "Guess AES Key": prob_AES_key,

    "Win Lottery Once": prob_lottery_1,

    "Win Lottery 5 Times": prob_lottery_5,

    "Win Lottery 6 Times": prob_lottery_6,

    "Win Lottery 7 Times": prob_lottery_7

}

  

# Sorting events by probability

sorted_events = sorted(events.items(), key=lambda x: x[1])

  

# Displaying the sorted events

sorted_events

```

  


# Lottery-Crypto

We compare the following events in terms of their likelihood:

1. Correctly guessing a random 128-bit AES key on the first try.
2. Winning a lottery with 1 million contestants.
3. Winning a lottery with 1 million contestants 5 times in a row.
4. Winning a lottery with 1 million contestants 6 times in a row.
5. Winning a lottery with 1 million contestants 7 times in a row.

  

## Calculated Probabilities

The probabilities of the events are:
- Correctly guessing a random 128-bit AES key: $\frac{1}{2^{128}}$
- Winning a lottery with 1 million contestants: $\frac{1}{10^6}$
- Winning a lottery with 1 million contestants 5 times in a row: $\left(\frac{1}{10^6}\right)^5$
- Winning a lottery with 1 million contestants 6 times in a row: $\left(\frac{1}{10^6}\right)^6$
- Winning a lottery with 1 million contestants 7 times in a row: $\left(\frac{1}{10^6}\right)^7$
