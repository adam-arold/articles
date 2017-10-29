> If you work in the IT industry you might have wondered about what technology do others use in the field and which tools are trending.
> There are a lot of resources where you can read about this like [Google Trends](https://trends.google.com/trends/)
> or [ITJobswatch](https://www.itjobswatch.co.uk) but what if you want more insight?
> Enter the [Stack Exchange Data Explorer](https://data.stackexchange.com/) where you can find fine grained information
> about **all** of the Stack Exchange users.

## So what is Stack Exchange?

> Launched in 2010, the Stack Exchange network consists of 133 Q&A communities including Stack Overflow,
> the largest, most trusted online community for developers to learn, share their knowledge, and build their careers.
>
> -- taken from the "about us" page of Stack Exchange

The [Stack Exchange Data Explorer](https://data.stackexchange.com/) is a meta-site where you can write queries against
**all** publicly available data in any of the Stack Exchange sites. In this article we'll work with the data from
Stack Overflow.

## Why is this useful to me?

If you look at the [queries](https://data.stackexchange.com/stackoverflow/queries) others created you can see a lot of
questions but most of them are related to Stack Overflow like "How many upvotes do I have for each tag?" or
"Jon Skeet comparison". These don't seem to be useful out of the context of the site itself, but you can define your
own queries by navigating to the [compose query](https://data.stackexchange.com/stackoverflow/query/new) page.

Let's look at a practical example!

Let's say that you have just moved to city `x` and you want to connect with other people in that area who are using
the same technology `y` you use. Stack Overflow aggregates users who are enthusiastic about their
craft so let's write a query which helps us find them.

> Note that Data Explorer comes with a very nice tutorial which you can check [here](https://data.stackexchange.com/tutorial)
if you are not familiar with the concepts detailed here.

If you navigate to the [compose query](https://data.stackexchange.com/stackoverflow/query/new) page you'll be presented
with something like this:

> image here

Here you can write *SQL* queries against the Stack Overflow database. On the right there are the tables which you can
query and in the center you can write the query itself. So let's write one:

```sql
select Id, Reputation, DisplayName, Location
from Users
where Location like '%Connecticut%'
order by Reputation desc
```

This will show us **all** Stack Overflow users who live in *Connecticut* ordered by their reputation.
If you want to be able to have a query which you don't need to modify by hand any time you want to change the Location
you can add a *parameter* to it. As shown in this query, you refer to a parameter by surrounding its name with doubled
pound signs:

```sql
select Id, Reputation, DisplayName, Location
from Users
where Location like '%##Location##%'
order by Reputation desc
```

When you run this SEDE will ask you to specify a Location. I suggest you should use a city name and only supply more
information (like `Connecticut, USA`) if you know that there are multiple cities with the name you like to use.

Now SEDE comes with another useful feature: it can put links into the results by using the syntax `[* Link]` like this:

```sql
select Id AS [User Link], Reputation, DisplayName, Location
from Users
where Location like '%##Location##%'
order by Reputation desc
```

If you try this you will see that the `id`s of the users are now links to their profile page.

So far so good but you only want to see users who have provided some kind of contact information on their profile, right?
It is quite easy to add to our existing query:

```sql
select u.Id AS [User Link], u.Reputation, u.DisplayName,
u.Location, u.WebsiteUrl
from Users u
where u.Location like '%##Location##%' and u.WebsiteUrl <> ''
order by u.Reputation desc
```

We can also filter for users who were active last month and have already contributed to Stack Overflow and have their
reputation above 1000:

```sql
select u.Id AS [User Link], u.Reputation, u.DisplayName,
u.Location, u.WebsiteUrl, u.LastAccessDate
from Users u
where u.Location like '%##Location##%'
  and u.WebsiteUrl <> ''
  and u.LastAccessDate > DATEADD(month, -1, GETDATE())
  and u.Reputation > 1000
order by u.Reputation desc, u.LastAccessDate desc
```

This is nice but we did not filter for technology so far. The *User*s on Stack Overflow have *Post*s and *Post*s can
have *Tag*s so let's add a parameter which will filter for a *Tag*:

```sql
select MAX(u.Id) AS [User Link], MAX(u.Reputation), MAX(u.DisplayName),
MAX(u.Location)
from Users u
  inner join Posts p on u.Id = p.OwnerUserId
where u.Location like '%##Location##%'
  and u.WebsiteUrl <> ''
  and u.LastAccessDate > DATEADD(month, -1, GETDATE())
  and u.Reputation > 1000
  and p.Tags <> ''
  and p.Tags like '%##Tag##%'
group by u.Id, u.Reputation, u.LastAccessDate
order by u.Reputation desc, u.LastAccessDate desc
```

And that's it! Now you can look for users who live in your specified city and have posts by your specified tag like this:

> image here

## What if I don't want to write SQL?

If you are a recruiter for example and you don't want to (or can't) write *SQL* and you just want quick answers for
some of the questions you have don't fret, there is a solution for your problem. SEDE comes with an useful search
function and if you try to look for something chances are that there is a prepared query for your problem. Let's look
at what we have if we search for the exact same thing I detailed above:

> image here

Most of these won't work out of the box so you have to try them but you'll get there eventually.

## Conclusion

In this article we explored how SEDE works and how can you extract useful information from it so go forth and try it out.
I'm interested in what you might get out from SEDE so you can share your insights in the comment section below if you'd like.

