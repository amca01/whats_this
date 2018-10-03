+++
title = "The power of two irrational numbers being rational"
author = ["Alasdair McAndrew"]
date = 2018-09-15
draft = false
mathjax = true
+++

There's a celebrated elementary result which claims that:

> There are irrational numbers \\(x\\) and \\(y\\) for which \\(x^y\\) is rational.

The standard proof goes like this.  Now, we know that \\(\sqrt{2}\\) is irrational,
so let's consider \\(r=\sqrt{2}^\sqrt{2}\\).  Either \\(r\\) is rational, or it is not.
If it is rational, then we set \\(x=\sqrt{2}\\), \\(y=\sqrt{2}\\) and we are done.  If
\\(r\\) is _irrational_, then set \\(x=r\\) and \\(y=\sqrt{2}\\).  This means that
\\[
x^y=\left(\sqrt{2}^\sqrt{2}\right)^{\sqrt{2}}=\sqrt{2}^2=2
\\]
which is rational.

This is a perfectly acceptable proof, but highly non-constructive,  And for some
people, the fact that the proof gives no information about the irrationality of
\\(\sqrt{2}^\sqrt{2}\\) is a fault.

So here's a lovely _constructive_ proof I found on [reddit](<https://www.reddit.com/r/math/comments/9i8lvl/classic/e6hnape>) .  Set \\(x=\sqrt{2}\\) and
\\(y=2\log\_2{3}\\).  The fact that \\(y\\) is irrational follows from the fact that if
\\(y=p/q\\) with \\(p\\) and \\(q\\) integers, then \\(2\log\_2{3}=p/q\\) so that \\(2^{p/2q}=3\\), or
\\(2^p=3^{2q}\\) which contradicts the fundamental theorem of arithmetic.  Then:

\begin{eqnarray\*}
x^y&=&\sqrt{2}^{2\log\_2{3}}\\\\\\
&=&2^{\log\_2{3}}\\\\\\
&=&3.
\end{eqnarray\*}

[//]: # "Exported with love from a post written in Org mode"
[//]: # "- https://github.com/kaushalmodi/ox-hugo"
