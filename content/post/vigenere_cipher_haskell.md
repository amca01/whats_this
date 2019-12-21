+++
title = "The Vigenere cipher in haskell"
author = ["Alasdair McAndrew"]
date = 2018-01-23
draft = false
mathjax = true
+++

Programming the Vigenère cipher is my go-to problem when learning a new
language. It's only ever a few lines of code, but it's a pleasant way of
getting to grips with some of the basics of syntax. For the past few
weeks I've been wrestling with [Haskell](https://www.haskell.org), and
I've now got to the stage where a Vigenère program is in fact pretty
easy.

As you know, the Vigenère cipher works using a plaintext and a keyword,
which is repeated as often as need be:

```text
T H I S I S T H E P L A I N T E X T
K E Y K E Y K E Y K E Y K E Y K E Y
```

The corresponding letters are added modulo 26 (using the values A=0,
B=1, C=2, and on up to Z=25), then converted back to letters again. So
for the example above, we have these corresponding values:

```text
19   7   8  18   8  18  19   7   4  15  11   0   8  13  19   4  23  19
10   4  24  10   4  24  10   4  24  10   4  24  10   4  24  10   4  24
```

Adding modulo 26 and converting back to letters:

```text
3  11   6   2  12  16   3  11   2  25  15  24  18  17  17
D   L   G   C   M   Q   D   L   C   Z   P   Y   S   R   R
```

gives us the ciphertext.

The Vigenère cipher is historically important as it is one of the first
cryptosystems where a single letter may be encrypted to different
characters in the ciphertext. For example, the two "S"s are encrypted to
"C" and "Q"; the first and last "T"s are encrypted to "D" and "R". For
this reason the cipher was considered unbreakable - as indeed it was for
a long time - and was known to the French as _le chiffre
indéchiffrable_ - the unbreakable cipher. It was broken in 1863. See the
[Wikipedia page](https://en.wikipedia.org/wiki/Vigenère%5Fcipher) for
more history.

Suppose the length of the keyword is . Then the -th character of the
plaintext will correspond to the character of the keyword (assuming a
zero-based indexing). Thus the encryption can be defined as

\\[
c\_i = p\_i+k\_{i\pmod{n}}\pmod{26}
\\]

However, encryption can also be done without knowing the length of the
keyword, but by shifting the keyword each time - first letter to the
end - and simply taking the left-most letter. Like this:

```text
T H I S I S T H E P L A I N T E X T
K E Y
```

so "T"+"K" (modulo 26) is the first encryption. Then we shift the
keyword:

```text
T H I S I S T H E P L A I N T E X T
  E Y K
```

and "H"+"E" (modulo 26) is the second encrypted letter. Shift again:

```text
T H I S I S T H E P L A I N T E X T
    Y K E
```

for "I"+"Y"; shift again:

```text
T H I S I S T H E P L A I N T E X T
      K E Y
```

for "S"+"K". And so on.

This is almost trivial in Haskell. We need two extra functions from the
module `Data.Char`: `chr` which gives the character corresponding to the
ascii value, and `ord` which gives the ascii value of a character:

```haskell
λ> ord 'G'
71
λ> chr 88
'X'
```

So here's what might go into a little file called `vigenere.hs`:

```haskell
import Data.Char (ord,chr)

vige :: [Char] -> [Char] -> [Char]
vige [] k = []
vige p [] = []
vige (p:ps) (k:ks) = (encode p k):(vige ps (ks++[k]))
  where
    encode a b = chr $ 65 + mod (ord a + ord b) 26

vigd :: [Char] -> [Char] -> [Char]
vigd [] k = []
vigd p [] = []
vigd (p:ps) (k:ks) = (decode p k):(vigd ps (ks++[k]))
  where
    decode a b = chr $ 65 + mod (ord a - ord b) 26
```

And a couple of tests: the example from above, and the one on the
Wikipedia page:

```haskell
λ> vige "THISISTHEPLAINTEXT" "KEY"
"DLGCMQDLCZPYSRROBR"
λ> vige "ATTACKATDAWN" "LEMON"
"LXFOPVEFRNHR"
```

[//]: # "Exported with love from a post written in Org mode"
[//]: # "- https://github.com/kaushalmodi/ox-hugo"
