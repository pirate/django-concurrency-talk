# How I Learned to Stop Worrying and Love atomic()
PyGotham 2018 talk about Banking Blunders and Concurrency Challenges in Django

[Nick Sweeting](https://nicksweeting.com) | [@theSquashSH](https://twitter.com/theSquashSH) on Twitter | [@pirate](https://github.com/pirate) on Github | [LinkedIn](https://www.linkedin.com/in/nick-sweeting-430999b3/) | Co-Founder/CTO @ [Monadical](https://monadical.com) building [OddSlingers Poker](https://labs.oddslingers.com)

---

<div style="text-align:center">
    <a href="https://pirate.github.io/django-concurrency-talk/slides.pdf">
        <img src="https://pirate.github.io/django-concurrency-talk/slides.jpg/slides.001.jpeg" alt="First slide of talk" width="500px">
     </a>
</div>

[**PyGotham Talk Page**](https://2018.pygotham.org/talks/how-i-learned-to-stop-worrying-and-love-atomic-banking-blunders-and-concurrency-challenges/)  
`#pygotham2018` `#python` `#django` `#distributed-systems` `#concurrency` `#databases`

(sorry for the overused "how I learned..." title)

## Video

(coming soon)

## Slides

You can view the slides here:

 - [PDF](./slides.pdf)
 - [Images](./slides.jpg/slides.001.jpeg)
 - [HTML](./slides.html)
 - [SlideShare](https://www.slideshare.net/NickSweeting1/how-i-learned-to-stop-worrying-and-love-atomic-banking-blunders-concurrency-challenges)
 - [SpeakerDeck](https://speakerdeck.com/pirate/how-i-learned-to-stop-worrying-and-love-atomic-banking-blunders-and-concurrency-challenges)


## Overview

What do Superman III, Hackers, and Office Space all have in common? Find out in this talk, along with some concurrency, database integrity, and financial data safety fundamentals. This is a technical talk going over the core principles in database integrity and data safety, it assumes familiarity with the Django ORM.

## Description

Did you know every Django app already behaves like a distributed system, even when it’s running only on one server? In this talk I’ll go over some of the distributed systems & database fundamentals that you’ll neeed to understand when building a Python project that handles sensitive data. We’ll focus on intermediate and advanced usage of the Django ORM, but many of the concepts apply equally well to SQLAlchemy and other Python ORMs.

We’ll go over:

 - how to use append-only logs to order events across your system
 - the meaning of transaction isolation levels
 - how and when to use atomic compare-and-swap operations
 - type safety for currencies
 - new distributed-SQL databases like spanner, TiDB, and Cockroachdb
 - transaction lifecycles when doing async processing with django-channels

We spent the last two years building an online [poker engine](https://labs.oddslingers.com/whitepaper.html) based on Django + channels, and we have plenty of stories about our failures and discoveries to share along the way. Come learn about all the ways it’s possible to screw up when handling sensitive data, and how to avoid them!

## Notes

One thing I didn't cover in the talk is that SQL can do something called "[gap locking](https://www.percona.com/blog/2012/03/27/innodbs-gap-locks/)".  That is if you have an index for a given column, and you perform a `.select_for_update()` with a filter, it won't just lock the rows that match the filter, it will actually prevent any new rows from being added that match the filter while the lock is held, which lets you effectively lock append-only tables without needing to lock the entire table.

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

## Further Reading

* Articles
    - [https://labs.oddslingers.com/posts/Designing-A-Banking-System.html](https://labs.oddslingers.com/posts/Designing-A-Banking-System.html)
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
