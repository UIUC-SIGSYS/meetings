---
title: "Plain Text"
---

## terminology

**Glyph** — a pictograph used to communicate information (letters, numbers, punctuation, etc.)

**Code point** — a numeric identifier assigned to a glyph or control character 

**Control character** — a code point without an associated glyph, used for conveying metadata

**Encoding** — a method of representing code points 

# Pre-Unicode

<style>
.kinda-small {
  font-size: 80%;
}
.small {
  font-size: 60%;
}
.large {
  font-szie: 200%;
}
.preserve-text {
  text-transform: none;
}
</style>

## ASCII

::::{.incremental}
- **A**merican **S**tandard **C**ode for **I**nformation **I**nterchange
- published in 1963, ASCII defines 128 code points, 95 of which are "printable"
- very limited set of punctuation which heavily influences the syntax of programming languages to present day
::::

## EBCDIC

::::{.incremental .kinda-small}
- **E**xtended **B**inary **C**oded **D**ecimal **I**nterchange **C**ode
- 256 code points encoded literally in 8 bits
- Used on IBM mainframes in the 50s and 60s 
- one critical flaw compared to ASCII, the alphabetic blocks weren't contiguous:
  - for example letters 'a' to 'i' had code points 0x81 to 0x89, but 'j' had code point 0x91 not 0x8a
  - this means many common programming tricks break on EBCDIC, such as:
  - `for (c = 'a'; c <= 'z'; ++c) putchar(c);`
  - `x - 'a'`
::::

# Unicode

## Unicode

::::{.incremental .kinda-small}
- Unicode is a set of code points from 0000 to 10FFFF hex
- Unicode code points are usually denoted U+XXXX(XX), such as U+1F4BB (💻) or U+028A (ʊ)
- there are currently 144,762 officially assigned code points, but large sections are reserved for "private use", or simply unallocated
- code points 0–127 are the same as ASCII for backwards compatibility
- Unicode includes many, *many* metadata tags to allow variation, combine characters, and control text rendering;
- a full breakdown on how to properly render Unicode is outside the scope of this presentation:
- the current version of the Unicode standard is **1,140** pages long
::::

# Character encodings

## 8-bit ASCII

::::{.incremental}
- ASCII has 128 code points; 00 through 7F hex 
- a byte has 8 bits; 00 through FF hex
- each ASCII code point `0xYZ` is encoded as `0b0YYYZZZZ`
::::

## UTF-32

::::{.incremental}
- the largest valid Unicode code point is 21-bits
- UTF-32 encodes each unicode code point as a fixed-size 32 bit word
- a code point U+uvwxyz is encoded in binary as: [`00000000 000uvvvv wwwwxxxx yyyyzzzz`]{.kinda-small}
::::

## examples

::::{.incremental}

::::{.fragment}
"E" — Latin Capital Letter E (U+0045)

[`00000000 00000000 00000000 01000101`]{.small}
::::

::::{.fragment}
"💻" — Personal Computer (U+1F4BB)

[`00000000 00000001 11110100 10111011`]{.small}
::::

::::

## pros and cons

::::{.incremental}
- unlike other Unicode encodings, UTF-32 is fixed size, meaning you can get the nth code point in O(1) instead of O(n)
- very wasteful of space:
  - every code point is padded with 11 zeros, so it has a best-case waste of 34% (1/4 of bytes)
  - in text composed entirely of ASCII code points (7 bits) it has a waste of 78% (3/4 bytes)
::::

## UTF-8

::::{.incremental .kinda-small}
- the most commonly used character encoding
- it uses variable width code points to compact smaller code points into less bytes compared to UTF-32
- this table shows how a code point U+uvwxyz is encoded in UTF-8:

::::{.fragment .small}
| First code point | Last code point | byte 1     | byte 2     | byte 3     | byte 4     |
| ---------------- | --------------- | ---------- | ---------- | ---------- | ---------- |
| U+0000           | U+007F          | `0yyyzzzz` |            |            |            |
| U+0080           | U+07FF          | `110xxxyy` | `10yyzzzz` |            |            |
| U+0800           | U+FFFF          | `1110wwww` | `10xxxxyy` | `10yyzzzz` |            |
| U+010000         | U+10FFFF        | `11110uvv` | `10vvwwww` | `10xxxxyy` | `10yyzzzz` |
::::

::::

## examples

::::{.incremental}

::::{.fragment}
"E" — Latin Capital Letter E (U+0045)

[`01000101`]{.small}
::::

::::{.fragment}
"💻" — Personal Computer (U+1F4BB)

[`11110000 10011111 10010010 10111011`]{.small}

[`XXXXX000 XX011111 XX010010 XX111011`]{.small .fragment}
::::

::::

## pros and cons

- backwards compatible with ASCII
- requires look-back to decode
- commonly used characters in European languages encode compactly

## UTF-16

::::{.incremental .kinda-small}
- okay, this one is weird
- code points U+0000 to U+D7FF and U+E000 to U+FFFF are encoded as the numeric form of their code points
- but code points U+D800 to U+DFFF of unicode are reserved for "surrogates":
  - code points U+010000 to U+10FFFF are encoded by subtracting 0x10000 from the code point
  - then the resultant 20-bit code point is split into two ten bit words: A and B
  - then 0xD800 is added to A and 0xDC00 is added to B
  - the character is then encoded in binary as A followed by B
::::

## surrogate example

![](./UTF-16_encoding.svg.png)

## pros and cons

::::{.incremental}
- not backwards compatible with ASCII
  - the Web Hypertext Application Technology Working Group (WHATWG) discourages its use for this reason;
  - there have been attacks on decoding implementations that exploit this fact to break input validators
- doesn't require lookback to decode
- can represent a lot of characters that take 3 bytes in UTF-8 in only 2
::::

# equivalence

## `strcmp(3)`

## simplified `strcmp`

```c
int strcmp(const char *s1, const char *s2) {
  for (size_t i = 0; s1[i] != '\0' || s2[i] != '\0'; ++i) {
    int x = s1[i] - s2[i];
    if (x != 0) {
      return x;
    }
  }
  return 0;
}
```

## the problem

::::{.incremental}
[here are some strings:]{.fragment}

::::{.large .fragment}
string 1: `"ĒÅö"`

string 2: `"ĒÅö"`
::::

[and here's the hex data of these strings (in UTF-8):]{.fragment}

::::{.large .fragment}
`C4 92 E2 84 AB C3 B6`

`45 CC 84 C3 85 6F CC 88 0A`
::::

[wut.]{.fragment}
::::

## string 1

- `Ē` — Latin Capital Letter E With Macron (U+0112)
- `Å` — Angstrom Sign (U+212B)
- `ö` — Latin Small Letter O With Diaeresis (U+00F6)

## string 2

<!-- if you're reading the source code, i used the non-combining versions in the slides so that they'd render properly on their own -->

- `E` — Latin Capital Letter E (U+0045)
- `¯` — Combining Macron (U+0304)
- `Å` — Latin Capital Letter A With Ring Above (U+00C5)
- `o` — Latin Small Letter O (U+006F)
- `¨` — Combining Diaeresis (U+0308)

## ✨ Unicode normalization ✨

Unicode defines 4 normal forms:

::::{.incremental .kinda-small}
- **NFD** — Normalization Form Canonical Decomposition
  - [Characters are decomposed by canonical equivalence, and multiple combining characters are arranged in a specific order.]{.small}
- **NFC** — Normalization Form Canonical Composition
  - [Characters are decomposed and then recomposed by canonical equivalence.]{.small}
- **NFKD** — Normalization Form Compatibility Decomposition
  - [Characters are decomposed by compatibility, and multiple combining characters are arranged in a specific order.]{.small}
- **NFKC** — Normalization Form Compatibility Composition
  - [Characters are decomposed by compatibility, then recomposed by canonical equivalence.]{.small}
::::

##

::::{.incremental}
- Unicode equivalence of strings can only be tested using byte-by-byte comparison if both strings are normalized into the same form and use the same encoding
- the all four algorithms for normalizing unicode are idempotent, but normalized unicode is not closed under string concatenation
::::

# in conclusion

## it's a mess
