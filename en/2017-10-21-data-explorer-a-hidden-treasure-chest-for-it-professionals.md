<div id="tldr">
If you work in the IT industry you might have wondered about what technology others use in the field and which tools are trending.
There are a lot of resources where you can read about this like <a href="https://trends.google.com/trends/">Google Trends</a>
or <a href="https://www.itjobswatch.co.uk">ITJobswatch</a> but what if you want more insight?
Enter the <a href="https://data.stackexchange.com/">Stack Exchange Data Explorer</a> where you can find fine grained information
about <strong>all</strong> of the Stack Exchange users.
</div>

## So what is Stack Exchange?

> Launched in 2010, the Stack Exchange network consists of 133 Q&A communities including Stack Overflow,
> the largest, most trusted online community for developers to learn, share their knowledge, and build their careers.
>
> --- taken from the "about us" page of Stack Exchange

The [Stack Exchange Data Explorer](https://data.stackexchange.com/) is a meta-site where you can write queries against
**all** publicly available data in any of the Stack Exchange sites. In this article we'll work with the data from
Stack Overflow.

## Why is this useful to me?

If you look at the [queries](https://data.stackexchange.com/stackoverflow/queries) others created you can see a lot of
questions but most of them are related to Stack Overflow like "How many upvotes do I have for each tag?" or
"Jon Skeet comparison". These don't seem to be useful out of the context of the site itself, but you can define your
own queries by navigating to the [compose query](https://data.stackexchange.com/stackoverflow/query/new) page.

## A practical example

Let's say that you have just moved to city `x` and you want to connect with other people in that area who are using
the same technology as you. Stack Overflow aggregates users who are enthusiastic about their
craft so let's write a query which helps us find them.

> Note that Data Explorer comes with a very nice tutorial which you can check [here](https://data.stackexchange.com/tutorial)
if you are not familiar with the concepts detailed here.

If you navigate to the [compose query](https://data.stackexchange.com/stackoverflow/query/new) page you'll be presented
with something like this:

![SEDE compose query]({{ site.url }}/assets/articles/sede_compose_query.png)

Here you can write *SQL* queries against the Stack Overflow database. On the right there are the tables which you can
query and in the center you can write the query itself. So let's write one:

{% gist 177231fb9b5d07c38c8ea979afd2b1fc %}

This will show us **all** Stack Overflow users who live in *Connecticut* ordered by their reputation.
If you want to be able to have a query which you don't need to modify by hand any time you want to change the Location
you can add a *parameter* to it. As shown in this query, you refer to a parameter by surrounding its name with doubled
pound signs:

{% gist 88c9de75b73e813d6d55cb928aa5f2c4 %}

When you run this, SEDE will ask you to specify a Location. I suggest you should use a city name and only supply more
information (like `Connecticut, USA`) if you know that there are multiple cities with the name you like to use.

Now SEDE comes with another useful feature: it can put links into the results by using the syntax `[* Link]` like this:

{% gist eeca1c9b26b9a1aabaead02ebf717b04 %}

If you try this you will see that the `id`s of the users are now links to their profile page.

So far so good but you only want to see users who have provided some kind of contact information on their profile, right?
It is quite easy to add to our existing query:

{% gist 1082f850430fc16c7bdb72a41ed14524 %}

We can also filter for users who were active last month and have already contributed to Stack Overflow and have their
reputation above 1000:

{% gist 1dcf009f8972f3e053f641c5a56f8506 %}

This is nice but we did not filter for technology so far. The *User*s on Stack Overflow have *Post*s and *Post*s can
have *Tag*s so let's add a parameter which will filter for a *Tag*:

{% gist be341df380f9926aa14abbc29f79e442 %}

And that's it! Now you can look for users who live in your specified city and have posts by your specified tag like this:

![SEDE query result]({{ site.url }}/assets/articles/sede_query_result.png)

## What if I don't want to write SQL?

If you are a recruiter for example and you don't want to (or can't) write *SQL* and you just want quick answers for
some of the questions you have don't fret, there is a solution for your problem. SEDE comes with an useful search
function and if you try to look for something chances are that there is a prepared query for your problem. Let's look
at what we have if we search for the exact same thing I detailed above:

![SEDE search result]({{ site.url }}/assets/articles/sede_search_result.png)

Most of these won't work out of the box so you have to try them but you'll get there eventually.

## Conclusion

In this article we explored how SEDE works and how can you extract useful information from it so go forth and try it out.
I'm interested in what you might get out from SEDE so you can share your insights in the comment section below if you'd like.

