#course_cs50

- How do we represent language in a way that can be understood by computers?
- We can assign numbers to each letter. For example, $65 \to \text{A}$; and this $65$ in turn is being represented by binary (`01000001`).
- This is ASCII (American Standard Code for Information Interchange).
    - This is for *English*, we can easily represent all characters in the English language (uppercase, lowercase, punctuation, numbers) using the 256 available representations a byte can provide
    - However, this isn't enough memory to represent some other languages, potentially accented characters, or emojis (which are just characters of an emoji alphabet). We can build on ASCII to address this using [[Unicode]].

| Decimal | Hex | Char |
| ------- | --- | ---- |
| 64      | 40  | @    |
| 65      | 41  | A    |
| 66      | 42  | B    |
| 67      | 43  | C    |
| 68      | 44  | D    |
| 69      | 45  | E    |
| 70      | 46  | F    |
| 71      | 47  | G    |
| 72      | 48  | H    |
|         |     |      |
|         |     |      |

