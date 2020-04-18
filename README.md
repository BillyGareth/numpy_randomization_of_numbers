# numpy_randomization_of_numbers
andomization with Real Numbers
This section describes randomization methods that use random real numbers, not just random integers.

However, whenever possible, applications should work with random integers, rather than other random real numbers. This is because:

Computers can represent integers more naturally than other real numbers, making random integer generation algorithms more portable and more numerically stable than random real number generation algorithms.(22)
No computer can choose from among all real numbers between two others, since there are infinitely many of them.
For applications that may care about reproducible "random" numbers (unit tests, simulations, machine learning, and so on), using non-integer numbers can complicate the task of making a method reproducible from run to run or across computers.(23)
The methods in this section should not be used to generate random numbers for information security purposes, even with a secure random number generator. See "Security Considerations" in the appendix.


Uniform Random Real Numbers
This section defines the following methods that generate independent uniform random real numbers:

RNDU01: Interval [0, 1].
RNDU01OneExc: Interval [**0, 1).(24)
RNDU01ZeroExc: Interval (0, 1].
RNDU01ZeroOneExc: Interval (0, 1).
RNDRANGE: Interval [a, b].
RNDRANGEMaxExc: Interval [a, b).
RNDRANGEMinExc: Interval (a, b].
RNDRANGEMinMaxExc: Interval (a, b).

For Fixed-Point Number Formats
For fixed-point number formats representing multiples of 1/n, these eight methods are trivial. The following implementations return integers that represent fixed-point numbers.

RNDU01 family:

RNDU01(): RNDINT(n).
RNDU01ZeroExc(): (RNDINT(n - 1) + 1) or (RNDINTEXC(n) + 1).
RNDU01OneExc(): RNDINTEXC(n) or RNDINT(n - 1).
RNDU01ZeroOneExc(): (RNDINT(n - 2) + 1) or (RNDINTEXC(n - 1) + 1).
RNDRANGE family. In each method below, fpa and fpb are the bounds of the random number generated and are integers that represent fixed-point numbers (such that fpa = a * n and fpb = b * n). For example, if n is 100, to generate a number in [6.35, 9.96], generate RNDRANGE(6.35, 9.96) or RNDINTRANGE(635, 996).

RNDRANGE(a, b): RNDINTRANGE(fpa, fpb).
RNDRANGEMinExc(a, b): RNDINTRANGE(fpa + 1, fpb), or an error if fpa >= fpb.
RNDRANGEMaxExc(a, b): RNDINTEXCRANGE(fpa, fpb).
RNDRANGEMinMaxExc(a, b): RNDINTRANGE(fpa + 1, fpb - 1), or an error if fpa >= fpb or a == fpb - 1.

RNDU01 Family: Random Numbers Bounded by 0 and 1
For the RNDU01 family, the list below shows different ways to implement each method, ordered from most preferred to least preferred. X and INVX are defined later.

RNDU01(), interval [0, 1]:
For Java float or double, use the alternative implementation given later.
RNDINT(X) * INVX.
RNDINT(X) / X, if the number format can represent X.
RNDU01OneExc(), interval [0, 1):
Generate RNDU01() in a loop until a number other than 1.0 is generated this way.
RNDINT(X - 1) * INVX.
RNDINTEXC(X) * INVX.
RNDINT(X - 1) / X, if the number format can represent X.
RNDINTEXC(X) / X, if the number format can represent X.
RNDU01ZeroExc(), interval (0, 1]:
Generate RNDU01() in a loop until a number other than 0.0 is generated this way.
(RNDINT(X - 1) + 1) * INVX.
(RNDINTEXC(X) + 1) * INVX.
(RNDINT(X - 1) + 1) / X, if the number format can represent X.
(RNDINTEXC(X) + 1) / X, if the number format can represent X.
1.0 - RNDU01OneExc() (but this is recommended only if the set of numbers RNDU01OneExc() could return — as opposed to their probability — is evenly distributed).
RNDU01ZeroOneExc(), interval (0, 1):
Generate RNDU01() in a loop until a number other than 0.0 or 1.0 is generated this way.
(RNDINT(X - 2) + 1) * INVX.
(RNDINTEXC(X - 1) + 1) * INVX.
(RNDINT(X - 2) + 1) / X, if the number format can represent X.
(RNDINTEXC(X - 1) + 1) / X, if the number format can represent X.
In the idioms above:

X is the highest integer p such that all multiples of 1/p in the interval [0, 1] are representable in the number format in question. For example—
for the 64-bit IEEE 754 binary floating-point format (e.g., Java double), X is 253 (9007199254740992),
for the 32-bit IEEE 754 binary floating-point format (e.g., Java float), X is 224 (16777216),
for the 64-bit IEEE 754 decimal floating-point format, X is 1016, and
for the .NET Framework decimal format (System.Decimal), X is 1028.
INVX is the constant 1 divided by X.
