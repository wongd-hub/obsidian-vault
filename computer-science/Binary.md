#course_cs50

## base-2

- Our everyday number system is a decimal-based (i.e. base-10) one.
    - Take the number 123 for example, we can think of this as 1 in the 100s slot, 2 in the 10s slot, 3 in the 1s slot; adding them we get 123
    - Generalising this, each of those slots is 10 to the power of something: $100 = 10^2, 10 = 10^1, 1 = 10^0$
- If we wanted to switch to a *binary* system, i.e. base-2, we would switch out those 10s for 2s. So the first three slots would be $4 = 2^2, 2 = 2^1, 1 = 2^0$
    - We could then represent numbers by placing 1s and 0s in each slot:

| Number | Binary Representation | Explanation             |
| ------ | --------------------- | ----------------------- |
| 1      | 001                   | $0 * 4 + 0 * 2 + 1 * 1$ |
| 2      | 010                   | $0 * 4 + 1 * 2 + 0 * 1$ |
| 6      | 110                   | $1 * 4 + 1 * 2 + 0 * 1$ |
| 7      | 111                   | $1 * 4 + 1 * 2 + 1 * 1$ |
| 8       | 1000                      |    $1 * 8 + 0 * 4 + 0 * 2 + 0 * 1$                     |

## Relevance to computer science

- How does this relate to computer science? Computers can only represent information in a binary way at the hardware level
    - The canonical example is a lightbulb; if it's off then it represents a 0, if it's on then it's a 1.
    - In a similar fashion, computers have millions of transistors that represent 1 by holding an electrical charge, or 0 by not holding one.

## Bits vs. bytes

- Each of the 'slots' above represents one *bit* (a shortened form of *binary digit*)
- 8 bits together form a byte, which can represent 256 numbers ($2^8$, $0 \to 255$)
- Files are therefore collections of these bytes, which can be thousands (kilobytes), millions (megabytes), billions (gigabytes), and so on