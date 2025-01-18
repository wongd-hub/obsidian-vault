#course_cs50 

- We've already seen [[RGB]] as a way to represent colours; however there's a standard notation for representing this information.
- We use hexadecimal (i.e. base-16) for this.

# The base-16 system

- In computers, we can represent the numbers from 10-15 using the first 6 letters of the alphabet.

```
0 1 2 3 4 5 6 7 8 9 A B C D E F
```

- Recall how [[Binary#base-2 | base-2]] works. base-16 works similarly, each place to the left is 16 raised to the power of the position.

| $16^1$ | $16^0$ |     | Value                     |
| ------ | ------ | --- | ------------------------- |
| 0      | 0      | =   | 0                         |
| 0      | 9      | =   | 9                         |
| 0      | F      | =   | 15                        |
| 1      | 0      | =   | 16                        |
| 1      | 1      | =   | 17                        |
| F      | F      | =   | 16 $\times$ 15 + 15 = 255 |
- With only 16 values in this system, we need 4 bits to represent each number ($log_{2}16$). Two hexadecimals together then makes 8 bits, i.e. 1 byte - a common unit of measurement.
- Being able to represent numbers between 0 and 255 means that we can represent each of the colour steps in RGB with 3 pairs of hexadecimals:

```
# <Red 0-16> <Green 0-16> <Blue 0-16>

e.g.

#FF0000 255 Red, 0 Green, 0 Blue
```

- Often we prefix hexadecimal numbers with `#` for colour codes and `0x` for most other purposes such as memory addresses.