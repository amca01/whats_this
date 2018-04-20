+++
title = "The trinomial theorem"
author = ["Alasdair McAndrew"]
date = 2018-04-05
tags = ["mathematics", "algebra"]
draft = false
mathjax = true
+++

When I was teaching the binomial theorem (or, to be more accurate, the binomial
_expansion_) to my long-suffering students, one of them asked me if there was a
_trinomial_ theorem.  Well, of course there is, although in fact expanding sums
of greater than two terms is generally not classed as a theorem described by the
number of terms. The general result is

\\[
(x\_1+x\_2+\cdots+x\_k)^n=\sum\_{a\_1+a\_2+\cdots+a\_k=n}
{n\choose a\_1,a\_2,\ldots,a\_k}x\_1^{a\_1}x\_2^{a\_2}\cdots x\_k^{a\_k}
\\]

so in particular a "trinomial theorem" would be

\\[
(x+y+z)^n=\sum\_{a+b+c=n}{n\choose a,b,c}x^ay^bz^c.
\\]

Here we define

\\[
{n\choose a,b,c}=\frac{n!}{a!b!c!}
\\]

and this is known as a _trinomial coefficient_; more generally, for an arbitrary
number of variables, it is a _multinomial coefficient_.  It is guaranteed to be
an integer if the lower values sum to the upper value.

So to compute \\((x+y+z)^5\\) we could list all integers \\(a,b,c\\) with \\(0\le a,b,c\le 5\\)
for which \\(a+b+c=5\\), and put them all into the above sum.

But of course there's a better way, and it comes from expanding \\((x+y+z)^5\\) as a
binomial \\((x+(y+z))^5\\) so that

\begin{array}{rcl}
(x+(y+x))^5&=&x^5\\\\\\
&&+5x^4(y+z)\\\\\\
&&+10x^3(y+z)^2\\\\\\
&&+10x^2(y+z)^3\\\\\\
&&+5x(y+z)^4\\\\\\
&&+(y+z)^5
\end{array}

Now we can expand each of those binomial powers:

\begin{array}{rcl}
(x+(y+x))^5&=&x^5\\\\\\
&&+5x^4(y+z)\\\\\\
&&+10x^3(y^2+2yz+z^2)\\\\\\
&&+10x^2(y^3+3y^2z+3yz^2+z^3)\\\\\\
&&+5x(y^4+4y^3z+6y^2z^2+4yz^3+z^4)\\\\\\
&&+(y^5+5y^4z+10y^3z^2+10y^2z^3+5yz^4+z^5)
\end{array}

Expanding this produces

\begin{split}
x^5&+5x^4y+5x^4z+10x^3y^2+20x^3yz+10x^3z^2+10x^2y^3+30x^2y^2z+30x^2yz^3\\\\\\
&+10x^2z^3+5zy^4+20xy^3z+30xy^2z^2+20xyz^3+5xz^4+y^5+5y^4z+10y^3z^2\\\\\\
&+10y^2z^3+5yz^4+z^5
\end{split}

which is an equation of rare beauty.

But there's a nice way of setting this up, which involves writing down Pascal's
triangle to the fifth row, and putting a fifth row, as a column, on the side.
Then multiply across:

\begin{array}{lcccccccccc}
1&&&&&&1&&&&&\\\\\\
5&&&&&1&&1&&&&\\\\\\
10\quad\times&&&&1&&2&&1&&&\\\\\\
10&&&1&&3&&3&&1&&\\\\\\
5&&1&&4&&6&&4&&1&\\\\\\
1&1&&5&&10&&10&&5&&1
\end{array}

to produce the final array of coefficients (with index numbers at the left):

\begin{array}{l\*{10}{c}}
0\qquad{}&&&&&&1&&&&&\\\\\\
1&&&&&5&&5&&&&\\\\\\
2&&&&10&&20&&10&&&\\\\\\
3&&&10&&30&&30&&10&&\\\\\\
4&&5&&20&&30&&20&&5&\\\\\\
5&1&&5&&10&&10&&5&&1
\end{array}

Row \\(i\\) of this array corresponds to \\(x^{5-i}\\) and all combinations of powers
\\(y^bz^c\\) for \\(0\le b,c\le i\\).  Thus for example the fourth row down,
corresponding to \\( i=3 \\), may be considered as the coefficients of the terms

\\[
x^2y^3,\quad x^2y^2z,\quad x^2yz^2,\quad xz^3.
\\]

Note that the triangle of coefficients is symmetrical along all three centre
lines, as well as rotationally symmetric by 120°.

[//]: # "Exported with love from a post written in Org mode"
[//]: # "- https://github.com/kaushalmodi/ox-hugo"
