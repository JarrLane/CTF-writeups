# wireword

For this challenge we are given this text file:

```
Another  one  got  caught   today,  it's  all  over    the   papers.  "Teenager  Arrested   in    Computer  Crime  Scandal",  "Hacker   Arrested    after    Bank  Tampering"...   Damn  kids.    They're  all   alike.  But    did  you,    in  your    three-piece   psychology    and    1950's   technobrain,    ever    take    a    look    behind   the  eyes    of  the   hacker?  Did  you  ever   wonder  what  made  him    tick,    what   forces  shaped  him,    what    may  have    molded   him?    I  am    a  hacker,   enter    my    world...    Mine    is    a   world    that  begins  with   school...  I'm  smarter  than    most    of   the  other  kids,    this    crap  they    teach   us  bores    me...    Damn    underachiever.    They're   all  alike.  I'm  in   junior  high  or    high    school.  I've    listened   to    teachers  explain    for  the   fifteenth    time    how    to    reduce    a   fraction.    I    understand    it.    "No,    Ms.   Smith,  I    didn't  show  my   work.  I  did  it    in  my  head..."    Damn
```
One thing you might automatically notice is that the spacing is very wierd, almost like it might be a sort of pattern.

Another thing to note is that the challenge name has the word wire in it, that almost sounds like morse code.

If you lay out the first few number of spaces for each word, you get 2, 2, 2, 3. When looking up a morse code guide we know s is three dots, and we know that the flag starts with s so thats one letter uncovered

We notice that there are either 2 3 or 4 spaces between the words throughout the text, we know 2 spaces represents a dot, 3 spaces is the end of a letter, and knowing that we can logically conclude that 4 spaces is a line.

To solve this challenge for some reason I decided to do it by hand but it would've probably been smarter to just use a python script, either way here is how I got the flag:

```
2223 S
22243 V
223 I
42223 B
4423 G
2423 R
2424243 I'm assuming {
443 M
444443 0
2423 r
2223 s
222443 3
2244243 _
42423 c
444443 0
4223 d
222443 3
2244243 _
244443 1
2223 s
2244243 _
42423 c
444443 0
444443 0
24223 l
2224224 I'm assuming }
```
