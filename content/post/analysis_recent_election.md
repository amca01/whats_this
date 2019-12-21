+++
title = "Analysis of a recent election"
author = ["Alasdair McAndrew"]
date = 2017-12-07
draft = false
mathjax = true
+++

On November 18, 2017, a by-election was held in my suburb of
[Northcote](https://en.wikipedia.org/wiki/Northcote,%5FVictoria), on
account of the death by cancer of the sitting member. It turned into a
two-way contest between Labor (who had held the seat since its inception
in 1927), and the Greens, who are making big inroads into the inner
city. The Greens candidate won, much to Labor's surprise. As I played a
small part in this election, I had some interest in its result. And so I
thought I'd experiment with the results and see how close the result
was, and what other voting systems might have produced.

In Australia, the voting method used for almost all lower house
elections (state and federal), is
[Instant Runoff
Voting](https://en.wikipedia.org/wiki/Instant-runoff%5Fvoting), also known as the Alternative Vote, and known locally as the
"preferential method". Each voter must number the candidates
sequentially starting from 1. All boxes must be filled in (except the
last); no numbers can be repeated or missed. In Northcote there were 12
candidates, and so each voter had to number the boxes from 1 to 12 (or 1
to 11); any vote without those numbers is invalid and can't be counted.
Such votes are known as "informal". Ballots are distributed according to
first preferences. If no candidate has obtained an absolute majority,
then the candidate with the lowest count is eliminated, and all those
ballots distributed according to their second preferences. This
continues through as many redistributions as necessary until one
candidate ends up with an absolute majority of ballots. So at any stage
the candidate with the lowest number of ballots is eliminated, and those
ballots redistributed to the remaining candidates on the basis of the
highest preferences. As voting systems go it's not the worst, although
it has many faults. However, it is too entrenched in Australian
political life for change to be likely.

Each candidate had prepared a
[How to Vote card](https://en.wikipedia.org/wiki/How-to-vote%5Fcard),
listing the order of candidates they saw as being most likely to ensure
a good result for themselves. In fact there is no requirement for any
voter to follow a How to Vote card, but most voters do. For this reason
the ordering of candidates on these cards is taken very seriously, and
one of the less savoury aspects of Australian politics is backroom
"preference deals", where parties will wheel and deal to ensure best
possible preference positions on other How to Vote cards.

Here are the 12 candidates and their political parties, in the order as
listed on the ballots:

Attention: The internal data of table "4" is corrupted!

For this election the How to Vote cards can be seen at the
[ABC
news site](http://www.abc.net.au/news/elections/northcote-by-election-2017/). The only candidate not to provide a full ordered list was
Joseph Toscano, who simple advised people to number his square 1, and
the other squares in any order they liked, along with a recommendation
for people to number Lidia Thorpe 2.

As I don't have a complete list of all possible ballots with their
orderings and numbers, I'm going to make the following assumptions:

1.  Every voter followed the How to Vote card of their preferred
    candidate exactly.
2.  Joseph Toscano's preference ordering is: 3,4,2,5,6,7,8,9,1,10,11,12
    (This gives Toscano 1; Thorpe 2; and puts the numbers 3 -- 12 in
    order in the remaining spaces).

These assumptions are necessarily crude, and don't reflect the nuances
of the election. But as we'll see they end up providing a remarkably
close fit with the final results.

For the exploration of the voting data I'll use Python, and so here is
all the How to Vote information as a dictionary:

```python
In [ ]: htv = dict()
	htv['Hayward']=[1,10,7,6,8,5,12,11,3,2,4,9]
	htv['Sanaghan']=[3,1,2,5,6,7,8,9,10,11,12,4]
	htv['Thorpe']=[6,9,1,3,10,8,12,2,7,4,5,11]
	htv['Lenk']=[7,8,3,1,5,11,12,2,9,4,6,10]
	htv['Chipp']=[10,12,4,5,1,6,7,3,11,9,2,8]
	htv['Cooper']=[5,12,8,6,2,1,7,3,11,9,10,4]
	htv['Rossiter']=[6,12,9,11,2,7,1,5,8,10,3,4]
	htv['Burns']=[10,12,5,3,2,4,6,1,11,9,8,7]
	htv['Toscano']=[3,4,2,5,6,7,8,9,1,10,11,12]
	htv['Edwards']=[2,10,4,3,8,9,12,6,5,1,7,11]
	htv['Spirovska']=[2,12,3,7,4,5,6,8,10,9,1,11]
	htv['Fontana']=[2,3,4,5,6,7,8,9,10,11,12,1]

	In [ ]: cands = list(htv.keys())
```

voting took place at different voting centres (also known as "booths"),
and the first preferences for each candidate at each booth can be found
at the
[Victorian
Electoral Commission](https://www.vec.vic.gov.au/Results/State2017/FPVbyVotingCentreNorthcoteDistrict.html). I copied this information into a spreadsheet and
saved it as a CSV file. I then used the data analysis library
[pandas](https://pandas.pydata.org) to read it in as a
[DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html):

```python
In [ ]: import pandas as pd
	firstprefs = pd.read_csv('northcote_results.csv')
	firsts = firstprefs.loc[:,'Hayward':'Fontana'].sum(axis=0)
	firsts

Out[ ]:
Hayward        354
Sanaghan       208
Thorpe       16254
Lenk           770
Chipp         1149
Cooper         433
Rossiter      1493
Burns        12721
Toscano        329
Edwards        154
Spirovska      214
Fontana       1857
dtype: int64
```

As Thorpe has more votes than any other candidate, then by the voting
system of simple plurality (or
[First Past
The Post](https://en.wikipedia.org/wiki/First-past-the-post%5Fvoting)) she would win. This system is used in the USA, and is
possibly the worst of all systems for more than two candidates.


## Checking IRV {#checking-irv}

So let's first check how IRV works, with a little program that starts
with a dictionary and first preferences of each candidate. Recall our
simplifying assumption that all voters vote according to the How to Vote
cards, which means that when a candidate is eliminated, all those votes
will go to just one other remaining candidate. In practice, of course,
those ballots would be redistributed across a number of candidates.

Here's a simple program to manage this version of IRV:

```python
def IRV(votes):
    # performs an IRV simulation on a list of first preferences: at each stage
    # deleting the candidate with the lowest current score, and distributing
    # that candidates votes to the highest remaining candidate
    vote_counts = votes.copy()
    for i in range(10):
	m = min(vote_counts.items(), key = lambda x: x[1])
	ind = next(j for j in range(2,11) if cands[htv[m[0]].index(j)] in vote_counts)
	c = cands[htv[m[0]].index(ind)]
	vote_counts += m[1]
	del(vote_counts[m[0]])
    return(vote_counts)
```

We could make this code a little more efficient by stopping when any
candidate has amassed over 50% pf the votes. But for simplicity we'll
eliminate 10 of the 12 candidates, so it will be perfectly clear who has
won. Let's try it out:

```python
In [ ]: IRV(firsts)
  Out[ ]:
  Thorpe    18648
  Burns     17288
  dtype: int64
```

Note that this is very close to the results listed on the VEC site:

```python
Thorpe:    18380
Burns:     14410
Fontana:   3298
```

At this stage it doesn't matter where Fontana's votes go (in fact they
would go to Burns), as Thorpe already has a majority. But the result we
obtained above with our simplifying assumptions gives very similar
values.

Now lets see what happens if we work through each booth independently:

```python
In [ ]: finals = {'Thorpe':0,'Burns':0}

In [ ]: for i in firstprefs.index:
   ...:     booth = dict(firstprefs.loc[i,'Hayward':'Fontana'])
   ...:     f = IRV(booth)
   ...:     finals['Thorpe'] += f['Thorpe']
   ...:     finals['Burns'] += f['Burns']
   ...:     print(firstprefs.loc[i,'Booth'],': ',f)
   ...:
Alphington :  {'Thorpe': 524, 'Burns': 545}
Alphington North :  {'Thorpe': 408, 'Burns': 485}
Bell :  {'Thorpe': 1263, 'Burns': 893}
Croxton :  {'Thorpe': 950, 'Burns': 668}
Darebin Parklands :  {'Thorpe': 180, 'Burns': 204}
Fairfield :  {'Thorpe': 925, 'Burns': 742}
Northcote :  {'Thorpe': 1043, 'Burns': 875}
Northcote North :  {'Thorpe': 1044, 'Burns': 1012}
Northcote South :  {'Thorpe': 1392, 'Burns': 1137}
Preston South :  {'Thorpe': 677, 'Burns': 639}
Thornbury :  {'Thorpe': 1158, 'Burns': 864}
Thornbury East :  {'Thorpe': 1052, 'Burns': 804}
Thornbury South :  {'Thorpe': 1310, 'Burns': 1052}
Westgarth :  {'Thorpe': 969, 'Burns': 536}
Postal Votes :  {'Thorpe': 1509, 'Burns': 2262}
Early Votes :  {'Thorpe': 5282, 'Burns': 3532}

In [ ]: finals
Out[ ]: {'Burns': 16250, 'Thorpe': 19686}
```

Note again that the results are surprisingly close to the
"[two-party
preferred](https://www.vec.vic.gov.au/Results/State2017/TCPbyVotingCentreNorthcoteDistrict.html)" results as reported again on the VEC site. This adds weight
to the notion that our assumptions, although crude, do in fact provide a
reasonable way of experimenting with the election results.


## Borda counts {#borda-counts}

These are named for
[Jean Charles de
Borda](https://en.wikipedia.org/wiki/Jean-Charles%5Fde%5FBorda) (1733 -- 1799) an early voting theorist. The idea is to weight
all the preferences, so that a preference of 1 has a higher weighting
that a preference of 2, and so on. All the weights are added, and the
candidate with the greatest total is deemed to be the winner. With
candidates, there are different methods of determining weighting;
probably the most popular is a simple linear weighting, so that a
preference of is weighted as . This gives weightings from down to zero.
Alternatively a weighting of can be used, which gives weights of down to

1.  Both are equivalent in determining a winner. Another possible

weighting is .

Here's a program to compute Borda counts, again with our simplification:

```python
def borda(x): # x is 0 or 1
    borda_count = dict()
    for c in cands:
	borda_count=0.0
    for c in cands:
	v = firsts  #  number of 1st pref votes for candidate c
	for i in range(1,13):
	    appr = cands[htv.index(i)]  # the candidate against position i on c htv card
	    if x==0:
		borda_count[appr] += v/i
	    else:
		borda_count[appr] += v*(11-i)
    if x==0:
	for k, val in borda_count.items():
	    borda_count[k] = float("{:.2f}".format(val))
    else:
	for k, val in borda_count.items():
	    borda_count[k] = int(val)
    return(borda_count)
```

Now we can run this, and to make our lives easier we'll sort the
results:

```python
In [ ]: sorted(borda(1).items(), key = lambda x: x[1], reverse = True)
Out[ ]:
[('Burns', 308240),
 ('Thorpe', 279392),
 ('Lenk', 266781),
 ('Chipp', 179179),
 ('Cooper', 167148),
 ('Spirovska', 165424),
 ('Edwards', 154750),
 ('Hayward', 136144),
 ('Fontana', 88988),
 ('Toscano', 80360),
 ('Rossiter', 75583),
 ('Sanaghan', 38555)]

In [ ]: sorted(borda(0).items(), key = lambda x: x[1], reverse = True)
Out[ ]:
[('Burns', 22409.53),
 ('Thorpe', 20455.29),
 ('Lenk', 11485.73),
 ('Chipp', 10767.9),
 ('Spirovska', 6611.22),
 ('Cooper', 6592.5),
 ('Edwards', 6569.93),
 ('Hayward', 6186.93),
 ('Fontana', 6006.25),
 ('Rossiter', 5635.08),
 ('Toscano', 4600.15),
 ('Sanaghan', 4196.47)]
```

Note that in both cases Burns has the highest output. This is in general
to be expected of Borda counts: that the highest value does not
necessarily correspond to the candidate which is seen as better overall.
For this reason Borda counts are rarely used in modern systems, although
they can be used to give a general picture of an electorate.


## Condorcet criteria {#condorcet-criteria}

There are a vast number of voting systems which treat the vote as
simultaneous pairwise contests. For example in a three way contest,
between Alice, Bob, and Charlie the system considers the contest between
Alice and Bob, between Alice and Charlie, and between Bob and Charlie.
Each of these contests will produce a winner, and the outcome of all the
pairwise contests is used to determine the overall winner. If there is a
single person who is preferred, by a majority of voters, in each of
their pairwise contests, then that person is called a _Condorcet
winner_. This is named for the
[Marquis de
Condorcet](https://en.wikipedia.org/wiki/Marquis%5Fde%5FCondorcet) (1743 -- 1794) another early voting theorist. The _Condorcet
criterion_ is one of many criteria considered appropriate for a voting
system; it says that if the ballots return a Condorcet winner, then that
winner should be chosen by the system. This is one of the faults of IRV:
that it does not necessarily return a Condorcet winner.

Let's look again at the How to Vote preferences, and the numbers of
voters of each:

```nil
In [ ]: htvd = pd.DataFrame(list(htv.values()),index=htv.keys(),columns=htv.keys()).transpose()
In [ ]: htvd.loc['Firsts']=list(firsts.values)
In [ ]: htvd

Out[ ]:
	   Hayward  Sanaghan  Thorpe  Lenk  Chipp  Cooper  Rossiter  Burns  Toscano  Edwards  Spirovska  Fontana
Hayward          1         3       6     7     10       5         6     10        3        2          2        2
Sanaghan        10         1       9     8     12      12        12     12        4       10         12        3
Thorpe           7         2       1     3      4       8         9      5        2        4          3        4
Lenk             6         5       3     1      5       6        11      3        5        3          7        5
Chipp            8         6      10     5      1       2         2      2        6        8          4        6
Cooper           5         7       8    11      6       1         7      4        7        9          5        7
Rossiter        12         8      12    12      7       7         1      6        8       12          6        8
Burns           11         9       2     2      3       3         5      1        9        6          8        9
Toscano          3        10       7     9     11      11         8     11        1        5         10       10
Edwards          2        11       4     4      9       9        10      9       10        1          9       11
Spirovska        4        12       5     6      2      10         3      8       11        7          1       12
Fontana          9         4      11    10      8       4         4      7       12       11         11        1
Firsts         354       208   16254   770   1149     433      1493  12721      329      154        214     1857
```

Here the how to vote information is in the columns. If we look at just
the first two candidates, we see that Hayward is preferred to Sanaghan
by all voters except for those who voted for Sanaghan. Thus a majority
(in fact, nearly all) voters preferred Hayward to Sanaghan.

For each pair of candidates, the number of voters preferring one to the
other can be computed by this program:

```python
def condorcet():
    condorcet_table = pd.DataFrame(columns=cands,index=cands).fillna(0)
    for c in cands:
	hc = htv
	for i in range(12):
	    for j in range(12):
		if hc[i] &lt; hc[j]:
		    condorcet_table.loc[cands[i],cands[j]] += firsts
    return(condorcet_table)
```

We can see the results of this program:

```python
In [ ]: ct = condorcet(); ct
Out[ ]:
	   Hayward  Sanaghan  Thorpe   Lenk  Chipp  Cooper  Rossiter  Burns  Toscano  Edwards  Spirovska  Fontana
Hayward          0     35728    4505   5042  19370   21633     20573   3116    35607     4888       3335    18283
Sanaghan       208         0    2065   2394  18648    3164     19926   2748     2835     2394       2394    17715
Thorpe       31431     33871       0  21504  20140   20935     34010  19370    33760    35428      32726    32153
Lenk         30894     33542   14432      0  19926   33442     34229   3886    33760    33935      32726    31945
Chipp        16566     17288   15796  16010      0   18895     34443   6037    18845    18404      18960    33871
Cooper       14303     32772   15001   2494  17041       0     34443   3395    18075    18404      15548    31608
Rossiter     15363     16010    1926   1707   1493    1493         0   4101    18075    18404      17041    15906
Burns        32820     33188   16566  32050  29899   32541     31835      0    35099    35428      32726    32024
Toscano        329     33101    2176   2176  17091   17861     17861    837        0     3887       2902    18075
Edwards      31048     33542     508   2001  17532   17532     17532    508    32049        0      20359    18075
Spirovska    32601     33542    3210   3210  16976   20388     18895   3210    33034    15577          0    20717
Fontana      17653     18221    3783   3991   2065    4328     20030   3912    17861    17861      15219        0
```

What we want to see, of course, if anybody has obtained a majority of
preferences against everybody else. To do this we can find all the
values greater than the majority, and add up their number. A value of 11
indicates a Condorcet winner:

```python
In [ ]: maj = firsts.sum()//2 + 1; maj
Out[ ]: 17969

In [ ]: ((ct &gt;= maj)*1).sum(axis = 1)
Out[ ]:
Hayward       6.0
Sanaghan      2.0
Thorpe       11.0
Lenk          9.0
Chipp         6.0
Cooper        5.0
Rossiter      2.0
Burns        10.0
Toscano       2.0
Edwards       5.0
Spirovska     6.0
Fontana       2.0
dtype: float64
```

So in this case we do indeed have a Condorcet winner in Thorpe, and this
election (at least with our simplifying assumptions) is also one in
which IRV returned the Condorcet winner.


## Range and approval voting {#range-and-approval-voting}

If you go to [rangevoting.org](http://rangevoting.org) you'll find a
nspirited defense of a system called _range voting_. To vote in such a
system, each voter gives an "approval weight" for each candidate. For
example, the voter may mark off a value between 0 and 10 against each
candidate, indicating their level of approval. There is no requirement
for a voter to mark candidates differently: a voter might give all
candidates a value of 10, or of zero, or give one candidate 10 and all
the others zero. One simplified version of range voting is approval
voting, where the voter simply indicates as many or as few candidates as
she or he approves of. A voter may approve of just one candidate, or all
of them. As with range voting, the winner is the one with the maximum
number of approvals. A system where each voter approves of just one
candidate is the First Past the Post system, and as we have seen
previously, this is equivalent to simply counting only the first
preferences of our ballots.

We can't possibly know how voters may have approved of the candidates,
but we can run a simple simulation: given a number between 1 and 12,
suppose that each voter approves of their first preferences. Given the
preferences and numbers, we can easily tally the approvals for each
voter:

```python
def approvals(n):
      # Determines the approvals result if voters took their
      # first n preferences as approvals
      approvals_result = dict()
      for c in cands:
	  approvals_result = 0
      firsts = firstprefs.loc[:,'Hayward':'Fontana'].sum(axis=0)
      for c in cands:
	  v = firsts  #  number of 1st pref votes for candidate c
	  for i in range(1,n+1):
	      appr = cands[htv.index(i)]  # the candidate against position i on c htv card
	      approvals_result[appr] += v
      return(approvals_result)
```

Now we can see what happens with approvals for :

```python
In [1 ]: for i in range(1,7):
    ...:     si = sorted(approvals(i).items(),key = lambda x: x[1],reverse=True)
    ...:     print([i]+[s[0] for s in si])
    ...:
[1, 'Thorpe', 'Burns', 'Fontana', 'Rossiter', 'Chipp', 'Lenk', 'Cooper', 'Hayward', 'Toscano', 'Spirovska', 'Sanaghan', 'Edwards']
[2, 'Burns', 'Thorpe', 'Chipp', 'Hayward', 'Fontana', 'Rossiter', 'Spirovska', 'Lenk', 'Edwards', 'Cooper', 'Toscano', 'Sanaghan']
[3, 'Burns', 'Lenk', 'Thorpe', 'Chipp', 'Hayward', 'Spirovska', 'Sanaghan', 'Fontana', 'Rossiter', 'Toscano', 'Edwards', 'Cooper']
[4, 'Burns', 'Lenk', 'Thorpe', 'Edwards', 'Chipp', 'Cooper', 'Fontana', 'Spirovska', 'Hayward', 'Sanaghan', 'Rossiter', 'Toscano']
[5, 'Thorpe', 'Lenk', 'Burns', 'Spirovska', 'Edwards', 'Chipp', 'Cooper', 'Fontana', 'Hayward', 'Sanaghan', 'Rossiter', 'Toscano']
[6, 'Lenk', 'Thorpe', 'Burns', 'Hayward', 'Spirovska', 'Chipp', 'Edwards', 'Cooper', 'Rossiter', 'Fontana', 'Sanaghan', 'Toscano']
```

It's remarkable, that after , the first number of approvals required for
Thorpe again to win is .


## Other election methods {#other-election-methods}

There are of course many many other methods of selecting a winning
candidate from ordered ballots. And each of them has advantages and
disadvantages. Some of the disadvantages are subtle (although
important); others have glaring inadequacies, such as first past the
post for more than two candidates. One such
[comparison
table](https://en.wikipedia.org/wiki/Comparison%5Fof%5Felectoral%5Fsystems) lists voting methods against standard criteria. Note that IRV --
the Australian preferential system -- is one of the very few methods to
fail monotonicity. This is seen as one of the system's worst failings.
You can see an example of this in an
[old
blog post](https://numbersandshapes.net/2010/07/mathematics-of-voting-3-electing-a-candidate-instant-runoff-voting/).

Rather than write our own programs, we shall simply dump our information
into the
[Ranked-ballot
voting calculator](http://www1.cse.wustl.edu/~legrand/rbvote/calc.html) page and see what happens. First the data needs to
be massaged into an appropriate form:

```python
In [ ]: for c in cands:
   ...:     st = str(firsts)+":"+c
   ...:     for i in range(2,13):
   ...:         st += "&gt;"+cands[htv.index(i)]
   ...:     print(st)
   ...:
354:Hayward&gt;Edwards&gt;Toscano&gt;Spirovska&gt;Cooper&gt;Lenk&gt;Thorpe&gt;Chipp&gt;Fontana&gt;Sanaghan&gt;Burns&gt;Rossiter
208:Sanaghan&gt;Thorpe&gt;Hayward&gt;Fontana&gt;Lenk&gt;Chipp&gt;Cooper&gt;Rossiter&gt;Burns&gt;Toscano&gt;Edwards&gt;Spirovska
16254:Thorpe&gt;Burns&gt;Lenk&gt;Edwards&gt;Spirovska&gt;Hayward&gt;Toscano&gt;Cooper&gt;Sanaghan&gt;Chipp&gt;Fontana&gt;Rossiter
770:Lenk&gt;Burns&gt;Thorpe&gt;Edwards&gt;Chipp&gt;Spirovska&gt;Hayward&gt;Sanaghan&gt;Toscano&gt;Fontana&gt;Cooper&gt;Rossiter
1149:Chipp&gt;Spirovska&gt;Burns&gt;Thorpe&gt;Lenk&gt;Cooper&gt;Rossiter&gt;Fontana&gt;Edwards&gt;Hayward&gt;Toscano&gt;Sanaghan
433:Cooper&gt;Chipp&gt;Burns&gt;Fontana&gt;Hayward&gt;Lenk&gt;Rossiter&gt;Thorpe&gt;Edwards&gt;Spirovska&gt;Toscano&gt;Sanaghan
1493:Rossiter&gt;Chipp&gt;Spirovska&gt;Fontana&gt;Burns&gt;Hayward&gt;Cooper&gt;Toscano&gt;Thorpe&gt;Edwards&gt;Lenk&gt;Sanaghan
12721:Burns&gt;Chipp&gt;Lenk&gt;Cooper&gt;Thorpe&gt;Rossiter&gt;Fontana&gt;Spirovska&gt;Edwards&gt;Hayward&gt;Toscano&gt;Sanaghan
329:Toscano&gt;Thorpe&gt;Hayward&gt;Sanaghan&gt;Lenk&gt;Chipp&gt;Cooper&gt;Rossiter&gt;Burns&gt;Edwards&gt;Spirovska&gt;Fontana
154:Edwards&gt;Hayward&gt;Lenk&gt;Thorpe&gt;Toscano&gt;Burns&gt;Spirovska&gt;Chipp&gt;Cooper&gt;Sanaghan&gt;Fontana&gt;Rossiter
214:Spirovska&gt;Hayward&gt;Thorpe&gt;Chipp&gt;Cooper&gt;Rossiter&gt;Lenk&gt;Burns&gt;Edwards&gt;Toscano&gt;Fontana&gt;Sanaghan
1857:Fontana&gt;Hayward&gt;Sanaghan&gt;Thorpe&gt;Lenk&gt;Chipp&gt;Cooper&gt;Rossiter&gt;Burns&gt;Toscano&gt;Edwards&gt;Spirovska
</pre>
```

The above can be copied and pasted into the given text box. Then the
page returns:

<style>
.basic-styling td,
.basic-styling th {
  border: 0px solid #999;
  padding: 0.25rem;
}
</style>

<div class="ox-hugo-table basic-styling">
<div></div>

| winner | method(s) |
|--------|-----------|
| Thorpe | Baldwin   |
|        | Black     |
|        | Carey     |
|        | Coombs    |
|        | Copeland  |
|        | Dodgson   |
|        | Hare      |
|        | Nanson    |
|        | Raynaud   |
|        | Schulze   |
|        | Simpson   |
|        | Small     |
|        | Tideman   |
| Burns  | Borda     |
|        | Bucklin   |

</div>

You can see that Thorpe would be the winner under almost every other
voting system. This indicates that Thorpe being returned by IRV seems
not just an artifact of the system, but represents the genuine wishes of
the electorate.

[//]: # "Exported with love from a post written in Org mode"
[//]: # "- https://github.com/kaushalmodi/ox-hugo"
