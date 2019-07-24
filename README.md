# Database Integrity in Django: Safely Handling Critical Data in Distributed Systems

*AKA: How I Learned to Stop Worrying and Love atomic(): Banking Blunders and Concurrency Challenges*  
**How to deal with concurrency, money, and log-structured data in the Django ORM**

Speaker Info: [Nick Sweeting](https://nicksweeting.com) | [@theSquashSH](https://twitter.com/theSquashSH) on Twitter | [@pirate](https://github.com/pirate) on Github | [LinkedIn](https://www.linkedin.com/in/nick-sweeting-430999b3/)  
Co-Founder @ Monadical, a startup based in Medellin, Colombia and Montreal, Canada with remote employees around the world ([we're hiring!](https://monadical.com/apply)).  
[Monadical](https://monadical.com) works on archiving the internet and making Esports more engaging by building [OddSlingers Poker](https://labs.oddslingers.com).

<a href="https://www.youtube.com/watch?v=VJSznZMvA1M"><img src="https://img.shields.io/badge/Watch-YouTube-red.svg?style=flat"/></a>
<a href="https://twitter.com/thesquashSH"><img src="https://img.shields.io/badge/Tweet-%40theSquashSH-lightblue.svg?style=flat"/></a>
<a href="https://github.com/pirate/django-concurrency-talk"><img src="https://img.shields.io/github/stars/pirate/django-concurrency-talk.svg?style=flat&label=Star+on+Github"/></a>

---

<div style="text-align:center">
    <a href="https://www.youtube.com/watch?v=rrlXAU-FGTo">
        <img src="https://i.imgur.com/Cm6Q2zX.jpg" alt="First slide of talk" width="500px">
     </a>
</div>
<a href="https://www.youtube.com/watch?v=rrlXAU-FGTo">Video (YouTube)</a>, <a href="https://pirate.github.io/django-concurrency-talk/slides.pdf">Slides (PDF)</a>

`#pyconco2019` `#pygotham2018` `#python` `#django` `#distributed-systems` `#concurrency` `#databases`

(sorry for the overused "how I learned..." title)

## Events & Video

- [**PyCon Colombia 2019**](https://www.pycon.co/en/talks/how-i-learned-to-stop-worrying-and-love-atomic-banking-blunders-and-concurrency-challenges/) @ Pontifical Xavierian University on 2019/04/08  
  Video: https://www.youtube.com/watch?v=rrlXAU-FGTo

- **[Django-NYC](https://www.meetup.com/nycpython/events/255468112/) & [NYC-Python](https://www.meetup.com/nycpython/events/255468112/) Talk Night** @ thelab NYC on 2018/10/25  
  Video: None available.

- [**PyGotham NYC 2018**](https://2018.pygotham.org/talks/how-i-learned-to-stop-worrying-and-love-atomic-banking-blunders-and-concurrency-challenges/) @ Hotel Pennsylvania NYC on 2018/10/06  
  Video: https://www.youtube.com/watch?v=VJSznZMvA1M

If you'd like to have me give this talk at one of your events, please reach out via Twitter or email!

## Slides

You can view the slides in several formats, though some may be more up-to-date than others:

 - [PDF](./slides.pdf)
 - [Images](./slides.jpg/slides.001.jpeg)
 - [HTML](./slides.html)
 - [Keynote](./slides.key)
 - [SlideShare](https://www.slideshare.net/NickSweeting1/how-i-learned-to-stop-worrying-and-love-atomic-banking-blunders-concurrency-challenges)
 - [SpeakerDeck](https://speakerdeck.com/pirate/how-i-learned-to-stop-worrying-and-love-atomic-banking-blunders-and-concurrency-challenges)

Contact me on Twiter [@theSquashSH](https://twitter.com/theSquashSH) for corrections or suggestions!  You're welcome to use content or portions of the talk with permission, as long as you give credit to both this talk and the relevant sources I cite.

## Overview

What do the plots of Superman III, Hackers, I love you Philip Morris, and Office Space all have in common? Find out in this talk, along with some concurrency, database integrity, and financial data safety fundamentals. This is a technical talk going over the core principles in database integrity and data safety, it assumes familiarity with the Django ORM.

## Description

Did you know every Django app already behaves like a distributed system, even when it’s running only on one server? In this talk I’ll go over some of the distributed systems & database fundamentals that you’ll neeed to understand when building a Python project that handles sensitive data. We’ll focus on intermediate and advanced usage of the Django ORM, but many of the concepts apply equally well to SQLAlchemy and other Python ORMs.

We’ll go over:

 - how to use immutable append-only logs to order events across your system (log-structured storage)
 - the importance of accurate timestamps, and why that's difficult in distributed systems
 - the meaning of transaction isolation levels
 - how and when to use locking (pessimistic concurrency)
 - how and when to use atomic compare-and-swaps (optimistic concurrency)
 - how and when to use a hybrid optimistic/pessimistic approach like MVCC
 - type safety for currencies, and math operations to avoid
 - new distributed-SQL databases like spanner, TiDB, and Cockroachdb
 - transaction lifecycles when doing async processing with django-channels

We spent the last two years building an online [poker engine](https://labs.oddslingers.com/whitepaper.html) based on Django + channels, and we have plenty of stories about our failures and discoveries to share along the way. Come learn about all the ways it’s possible to screw up when handling sensitive data, and how to avoid them!

This is half overview of distributed systems fundamentals, half tales of our adventures at OddSlingers designing a django-channels backend to power an online poker platform. We’ll cover some common financial data pitfalls, get into some more advanced nuances of transactions, and discuss what modern “next generation distributed SQL” databases like CockroachDB and TiDB do, and how it all ties into to building a globally distributed apps. We’ll also cover the benefits of log-structured data and how it makes reasoning about stateful systems easier.

Did you know every Django app is already a distributed system, even when it’s running only on one server? Have you heard of .select_for_update() and transaction.atomic()? Did you know that even when using them, there are at least 3 different ways you can encounter DB consistency bugs? Come learn about all the ways it’s possible to screw up when handling sensitive data, and how to avoid them!

## Background

This talk expects a basic familiarity with the Django ORM, but doesn’t require that people know about transactions, atomicity, distributed SQL, or any of the other nitty-gritty details. I’m going to be giving many examples to illustrate each bug, and talk about the real-world effects. I expect beginners and advanced Django users will all learn at least 1 new thing in this talk, but it’s primarily aimed at intermediate developers who are new to thinking about modeling concurrency and distributed systems.

I’ve previously given lighting talks and spoken publicly about a wide range of technical topics at meetups in NYC, SF, Medellin, and Shanghai. I’m giving this talk as the CTO of Monadical (https://monadical.com), where we do real-time poker (and side-betting, on the blockchain, woo buzzwords!). We’ve learned tons of lessons over the last two years working with django, django-channels, React/Redux, and sensitive financial data, and we hope to share plenty of fun stories about the poker world, and about building real-time systems in django!

I gave this talk at PyGotham 2018 recently, but I’ve since added significant improvements and new slides after speaking with several advanced Django users in the audience.

## Speaker Bio

I dropped out of high school in Shanghai to start coding, and have been founding and working at startups in San Francisco, Portland, Montreal, and NYC ever since.  I've been working with Django since my first under-the-table gig for a Shanghainese food-delivery startup, but have carried those skills with me from the healthcare and agriculture industries, to now the online gaming and blockchain worlds by cofounding a poker startup in Medellín called OddSlingers.  I love making mistakes and asking stupid questions, it's the best way to learn!

I also love learning about distributed systems and doing security activism, you may have heard of me as the "Equifax guy" who made the NYTimes for creating a hoax phishing site that Equifax accidentally linked millions of customers to.

## Addendum

### Timestamp Accuraccy

One thing I wish I spent more time on in the talk is the importantce of timestamp accuracy.  Putting all your db writes in a queue with timestamps is not nearly enough in a distributed system to guarantee serializability with globally correct ordering, you also need to guarantee the timestamps are monotonically increasing and ordered across all servers (which means using atomic clocks or requesting them from a central counter/timestamp server instead of generating them locally and relying on NTP).

### More on Decimal

#### Why floats are bad

‼️ Native Python math behavior with `float`s and `int`s is intuitive, but can be dangerous for money
because it hides the effects of imperfect precision from the programmer.

```python
>>> (1/3) * 100
33.33333333333333
>>> (1/3) * 100.0
33.33333333333333
>>> Decimal((1/3) * 100)
Decimal('33.3333333333333285963817615993320941925048828125')  # Not the number you though you had, eh?
```

#### Instantiating `Decimal`s with `float`s by accident

⚠️ When defining literal `Decimal`s, you **must pass a string literal**, otherwise it gets interpreted as a `float` first (which breaks the whole point of using `Decimal`).

```python
>>> from decimal import Decimal
>>> Decimal(0.1)    # bad
Decimal('0.1000000000000000055511151231257827021181583404541015625')
>>> Decimal('0.1')  # good
Decimal('0.1')
```
#### Irrational Numbers (aka infinite decimals)

ℹ️ If you need to represent an infinitely repeating decimal like `33.333333...%` or something like Pi `3.141...`, `Decimal` is not enough because it cannot store an infinite number of digits.  
For perfect precision when dealing with fractional values, you'll want to use `from fractions import Fraction` instead.

```python
>>> from fractions import Fraction
>>> Fraction(1) / Fraction(3)
Fraction(1, 3)
```

#### Implicit type conversion during match

⚠️ Watch out for implicit type conversion when doing math with `Decimal` and `Fraction` values.  
Make sure **all** values in math operations are `Decimal`s or `Fraction`s to maintain perfect precision.  

Don't multiply `Decimal('0.3') * 100`, do `Decimal('0.3') * Decimal('100')` instead.

This even more true when using `Fraction`. The result from mixed type math is not intuitive because of differening behavior
depending on which implicit type conversion takes place.
  
A naive implementation might try to do simple math `Fraction` and `int` (different types), but not 
only is the final value produced incorrect, the intermediate result of the multiplication before it's 
converted to float is actually stored incorrectly as well.  
  
This type of bug is subtle and extremely dangerous, because `Fraction` will blindly store the imperfect result of 
the multiplication with perfect precision. The type of the final value `Fraction` will imply 
to developers that it's correct and precsice, but in reality it's hiding float error that was introduced in an intermediate step that involved implicit type conversion.

```python
# implicit type conversion, even with simple ints ruins precision and introduces error
>>> float((Fraction(1) / Fraction(3)) * 100)
33.333333333333336

# don't try to fix this with float, it's secretly hiding the precision error instead of handling it properly
>>> float((Fraction(1) / Fraction(3)) * 100.0)
33.33333333333333
>>> Decimal((Fraction(1) / Fraction(3)) * 100.0
Decimal('33.3333333333333285963817615993320941925048828125')  # error can be revealed with Decimal
```

✅ **The correct apporach with `Decimal`:**  (don't use with irrational numbers / infinite decimals)
```python
>>> (Decimal(1) / Decimal(3)) * Decimal(100)
Decimal('33.33333333333333333333333333')      # Note this does not store infinite precision unlike Fraction, but it will provide correct results up to the Decimal precision limit
``` 

✅ **The correct apporach with `Fraction`:** (safe for irrational numbers / infinite decimals)
```python
>>> frac = (Fraction(1) / Fraction(3)) * Fraction(100)
>>> frac
Fraction(100, 3)                                # this is both stored and displayed correclty with infinite precision, unlike Decimal

>>> frac.numerator / Decimal(frac.denominator)  # if you want to get the value as a Decimal (correctly stored and displayed up to the Decimal precision limit)
Decimal('33.33333333333333333333333333')
``` 

### SQL Gap-Locking

One thing I didn't cover in the talk is that SQL can do something called "[gap locking](https://www.percona.com/blog/2012/03/27/innodbs-gap-locks/)".  That is if you have an index for a given column, and you perform a `.select_for_update()` with a filter, it won't just lock the rows that match the filter, it will actually prevent any new rows from being added that match the filter while the lock is held, which lets you effectively lock append-only tables without needing to lock the entire table.

(Thanks to Sam Kimbrel for telling me about this feature in the hall after the talk)

Example:

```python
class BalanceTransfer(models.Model):
    src = models.ForeignKey(User)
    dst = models.ForeignKey(User)
    amt = models.DecimalField(max_digits=20, decimal_places=2)
    timestamp = models.DateTimeField(auto_now_add=True)

...
with transaction.atomic():
    lock = BalanceTransfer.objects.select_for_update().filter(Q(src=user) | Q(dst=user))
    ...
    # no new rows can be added matching Q(src=user) | Q(dst=user) during this time
    # meaning we can safely do some logic without a user's balance being changed by new transfers added to the table
    withdraw(user, 5000)
```

Gap locking only works on columns that are indexed (all columns must be indexed together for the query you're tring to lock).  By default, all django ForeignKey columns are indexed, so you only need to worry about that if you want to gap lock a field like `CharField`, `DecimalField`, `DateTimeField`, etc.

## Further Reading

If you want to learn more about Django, databases, concurrency and data integrity, check out these talks and articles that go into more depth.

* Articles
    - [https://labs.oddslingers.com/posts/Designing-A-Banking-System.html](https://labs.oddslingers.com/posts/Designing-A-Banking-System.html)
    - [https://medium.com/@hakibenita/how-to-manage-concurrency-in-django-models-b240fed4ee2](https://medium.com/@hakibenita/how-to-manage-concurrency-in-django-models-b240fed4ee2)
    - [https://blogs.oracle.com/oraclemagazine/on-transaction-isolation-levels](https://blogs.oracle.com/oraclemagazine/on-transaction-isolation-levels)
    - [https://jepsen.io/consistency](https://jepsen.io/consistency)
    - [https://medium.com/abrai/demystifying-consistency-and-isolation-for-a-distributed-systems-engineer-64a064c52f6e](https://medium.com/abrai/demystifying-consistency-and-isolation-for-a-distributed-systems-engineer-64a064c52f6e)
    - [https://medium.com/@tylerneely/fear-and-loathing-in-lock-free-programming-7158b1cdd50c](https://medium.com/@tylerneely/fear-and-loathing-in-lock-free-programming-7158b1cdd50c)
    - [https://github.com/danluu/post-mortems](https://github.com/danluu/post-mortems)
    - [https://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying](https://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying)
    - [https://www.confluent.io/blog/turning-the-database-inside-out-with-apache-samza/](https://www.confluent.io/blog/turning-the-database-inside-out-with-apache-samza/)
    - [https://archive.sweeting.me/archive/1536044832/docs.djangoproject.com/en/2.1/topics/db/optimization.html](https://archive.sweeting.me/archive/1536044832/docs.djangoproject.com/en/2.1/topics/db/optimization.html)
    - [https://archive.sweeting.me/archive/1510783392/fauna.com/blog/consistent-transactions-in-a-globally-distributed-database.html](https://archive.sweeting.me/archive/1510783392/fauna.com/blog/consistent-transactions-in-a-globally-distributed-database.html)
    - [https://medium.com/@roman01la/on-web-apps-and-databases-c026f77b93f4](https://medium.com/@roman01la/on-web-apps-and-databases-c026f77b93f4)
    - [martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html](http://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)

* Talks
    - [https://pyvideo.org/pybay-2016/django-channels-and-distributed-systems.html](https://pyvideo.org/pybay-2016/django-channels-and-distributed-systems.html)
    - [https://reinout.vanrees.org/weblog/2014/05/13/distributed-systems.html](https://reinout.vanrees.org/weblog/2014/05/13/distributed-systems.html)
    - [https://2017.djangocon.us/talks/taking-django-distributed/](https://2017.djangocon.us/talks/taking-django-distributed/)
    - [https://speakerdeck.com/andrewgodwin/scaling-django-with-distributed-systems](https://speakerdeck.com/andrewgodwin/scaling-django-with-distributed-systems)

If any of these links are broken, check https://archive.sweeting.me for mirror copies.
