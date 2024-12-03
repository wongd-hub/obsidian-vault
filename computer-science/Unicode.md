#course_cs50

- Unicode is a _superset_ of ASCII as we realised we need more bits to represent languages, emojis, etc.
    - Unicode's mission is to represent and preserve all human language both past, present, and future.
- Unicode can use 8-32 bits (1-4 bytes) of information, allowing us to represent up to 4 billion characters.

> [!tip] A note on emojis
> Emojis look different across different platforms (MacOS vs. Windows), and this is because their unicode represents their _character_, however these characters can be drawn differently. Think of it like different fonts for emoji sets.

- However, with such large collections of bits, unicode is represented slightly differently: Unicode code points - which use base-16 (hexadecimal).
    - By convention, all unicode code points start with `U+` which clue the computer in to the fact that its about to read unicode.

| Code    | Emoji |
| ------- | ----- |
| U+1F600 | 😀    |
| U+1F603 | 😃    |
| U+1F604 | 😄    |
| U+1F601 | 😁    |
| U+1F606 | 😆    |
- What if we want to customise an emoji; i.e. giving a skin tone to each emoji? This suggests that we have to significantly increase how many new patterns of 0s and 1s we need to use.
    - However, we can think of these skin tones as _modifiers_ on a base version of the emoji.
    - So we use the same unicode code point, and add another switch that adjusts the emoji to a different version. This means we use a bit more memory, but increases our options drastically.

| Code            | Emoji |
| --------------- | ----- |
| U+1F44B U+1F3FB | 👋🏻  |
| U+1F44B U+1F3FC | 👋🏼  |
| U+1F44B U+1F3FD | 👋🏽  |
| U+1F44B U+1F3FE | 👋🏾  |
| U+1F44B U+1F3FF | 👋🏿  |
- What about combination emojis? We can combine multiple unicode code points in standardised combinations, delimited by `U+200D` which represents a [unicode zero width joiner](https://unicode-explorer.com/c/200D) (ZWJ, zwidge).

| Code                                   | Emoji     | Parts     |
| -------------------------------------- | --------- | --------- |
| U+1FAF1 U+1F3FF U+200D U+1FAF2 U+1F3FD | 🫱🏿‍🫲🏽 | 🫱🏿 🫲🏽 |
| U+1F9D4 U+200D U+2642 U+FE0F           | 🧔‍♂️     | 🧔 ♂      |
| U+1F9D1 U+1F3FB U+200D U+1F37C         | 🧑🏻‍🍼   | 🧑🏻 🍼   |



