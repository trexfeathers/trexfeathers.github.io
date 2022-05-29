## Data Analyst at Thrifty

In January 2016 I started a job at Thrifty Car and Van Rental in analytics team
of the Fleet department. A small team of us were responsible for a variety of
Excel files built on Power Pivot data models (DAX). These files queried the
company’s T-SQL databases for data on the maintenance and rental of the vehicle
fleet, and were used for generating several routine reports, answering daily
adhoc questions and also empowering other colleagues to perform their own adhoc
investigations

While PowerPivot was being used, there had been little effort to explore its
full potential. I made a point to get up to speed with this technology as
quickly as possible, and was soon able to offer up pivot-able measures that were
previously only available through manual preparation. This greatly enhanced the
reactiveness of the department by expanding the ‘self-serve’ analysis that could
be performed without needing to involve the analytics team

Almost every file that was in regular use had originated from a one-off report
request, but with no further resource investment, often leaving the file needing
various manual interventions to keep it working. This had been the same story in
my previous job, and having recognised this repeat pattern I insisted wherever
possible that we take extra time to create flexible solutions, which would be
able to serve both the initial report requirement and also answer subsequent
questions/requirements

For several of our routine reports, the team was tasked with analysing potential
causes for any observed trends. This was done by looking for other correlated
shifts in numbers and exploring whether they could be explanatory, or whether
both shifts came from a shared underlying cause. For some of the larger datasets
I created a VBA script to show all possible correlations. But as we did more of
this analysis it became clear that managers within the department often already
knew an explanation it took us hours to work out. The right thing to do was to
stop focussing on our own analysis but instead put more effort into helping
augment the work begin done by experts within each team. This meant creating
more flexible data models that could be pivoted in many different ways depending
on the investigation, and helping others with interpreting data (I often needed
to help people
avoid [Simpson’s Paradox](https://en.wikipedia.org/wiki/Simpson%27s_paradox) for
example)

As with many growing companies, one of the struggles with reporting was that
several of the systems in use were third party and it could be difficult to get
access to the data. For rich analysis we needed to be able to link data from
different systems together via SQL queries against the data warehouse, but for
one of the key systems data was only updated overnight, limiting the insight one
could gain from it. Arranging more frequent data feeds with the the third party
would have been expensive and also inflexible if our needs changed. But the
system did come with a proprietary querying framework, and it was down to me to
learn my way around and set up the necessary queries that allowed hourly updates
into the data warehouse. Not only has this proved very useful to stakeholders
but has also added a level of redundancy to the data warehousing process, of
which it is still a core part today

---

The close work with the IT department throughout this gave me the chance to
transfer into IT's data team mid-2017. This job entailed managing a much wider
range of reports around the business, as well as looking after data integration
and data warehousing between the myriad of systems in use

Apart from the manager we had minimal previous experience in our team of 4 so it
was important to focus on up-skilling as quickly as possible. Knowing I learn
best while on the job, I began taking on completely unfamiliar tasks to immerse
myself in as much of the team's responsibilities as possible. My growing
familiarity with the job, and willingness to investigate anything I didn't
understand, very soon started to ease pressure on the manager allowing them to
take a step back from the technical side of things and oversee the team as a
whole

For any concepts that weren't known to the whole team, I took time away from my
own tasks to work in parallel with my colleagues and pass on the knowledge. The
stronger the team's knowledge-base, the more flexible we were able to be with
sharing tasks between us, giving us a faster turnaround on user requests

A few months after the team formed, we were tasked with putting together a suite
of reports for an external customer. The specification was challenging and the
timeline short, but through clever distribution and prioritisation of this and
other tasks we were able to keep a constant focus on the project. We all had a
good instinct for aesthetics, and through persistence were able to make our
designs a reality despite the limitations of SSRS. The wider IT department were
impressed with both the visual quality and fast development of these reports,
which were of a quality they had not seen before

A big obstacle for any team, but especially an inexperienced one, is making sure
each member can pick up another's work - it would be just too time inefficient
to make sure everyone was constantly familiar with everyone else's reports/code.
With this in mind I constantly pushed for more robustness in a number of ways.
For instance: when creating a report using a visual editor (e.g. SSRS), it is
much harder to be aware of all background expressions than it is to review all
the SQL that feeds the report, so wherever possible it would be better to
transfer the expressions into the SQL instead. In a similar vein, if code within
SQL must be repeated there is a chance that future modifications will forget to
modify all repetitions, so it is much safer to abstract this code using
subqueries/temp tables/`CROSS APPLY`. And if there is still a chance of a lack
of clarity, it is incredibly important to comment code and document the wider
process

During a restructure our team took on first line support for reporting and data
integration, which meant learning the company's user ticketing system. I was
keen to harness this as our existing handling of this support was a sometimes
confusing mix of phone calls, forwarded tickets and Cc'd emails. I liaised with
the support team and experimented with filters and categories to make sure the
right tickets reached our team as quickly as possible. The end result was a
smoother sharing of work within the team, and a satisfying reduction of
scepticism on the users' part as they could see that sending in tickets was just
as effective as a phone call. The entire team became familiar with managing our
support queue as needed, again increasing our robustness
