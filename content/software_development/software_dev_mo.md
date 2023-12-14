## Software Developer at the Met Office

In 2019 I joined the UK Met Office, developing and supporting software for
analysing weather and climate data. Our team helped scientists of all kinds use
the open source Python ecosystem to enhance the forecast and perform research.
We did this in three ways:

* Maintaining a collection of open source libraries to support specialist user
  needs - see [scitools.org.uk](https://scitools.org.uk/)
* Provisioning environments to make ours and other valuable Python packages
  easily available as a 'common toolkit' across the Office.
* Supplying expertise to the Office's Python user community, including many
  novices.

#### Learning something completely new

Unlike all previous roles, I had no existing grounding in the tools used. Python
was entirely new for me, and even object-oriented programming was fairly
unfamiliar (VBA being merely an enhancement to existing Excel functionality).
Git and GitHub were also markedly different from my previous source control
experience (in a good way!). Getting up to speed as quickly as possible meant
swallowing pride and being willing to ask questions, no matter how basic, no
matter how big the group. I looked for any opportunities to frame my learning in
the forms that work best for me - primarily via solving practical problems.

#### Fresh input to the team

The team was welcoming and open to my input even as a newcomer, so I made 
sure to be as forthcoming as possible while my perspective was fresh - this 
window is always a short one. This approach proved particularly
valuable in the sphere of user support; from my previous role I was able to 
share many patterns and anti-patterns about fostering trust in one's user-base.

#### Engaging the user community

I quickly became the team's main advocate for user engagement. User support 
was often seen as a chore, to be cleared as quickly as possible before returning
to development work (a cliché in software development). I pushed for a more 
strategic attitude; time with users is an opportunity to:

* Learn how people truly relate to your product/service
* Build faith in your service
* Nurture collaborative relationships

I demonstrated how I saw this as a virtuous cycle where the outcomes of more 
support help us deliver increased value for less work, encouraging more 
users, some of whom will need support. This was especially important when 
producing something where users and developers each provided a share of the 
total expertise - we only had the full set by working together!

My championing in this area resulted in more resource being given to 
support and the team being seen as more approachable, and this did indeed 
uncover new opportunities for improvement.

#### Making routine tasks easier

The team had a number of routine administrative tasks to achieve various things
including: organising walk-in 'surgeries', tracking who we had given support,
and sharing tips/tricks as we discovered them. These were complex enough to be a
significant interruption to team members' workflows, while their nature
prevented full automation. I focussed on the next best option: making them as
simple as possible. I invested time in learning how we could get the best from
Microsoft 365, including SharePoint lists, Forms and Power Automate. I also
created checklists wherever we had a series of rote steps. The result allowed
the team to deliver the same value but with much less complexity /
context-switching.

#### Creative development

9 months after starting, the team took on a project to adapt our main 
software library - [Iris](https://github.com/SciTools/iris) - to handle data
located on an unstructured mesh. This broke the mould compared to 'typical' 
software projects:

* Not incremental, nor something completely new, but instead integrating a 
  new concept into an existing library
* No user cases to help structure the work - we were preparing functionality 
  for users that would appear in future
* Minimal global expertise in using unstructured meshes for atmospheric 
  science. We were among the first to think about these problems

This was therefore an unusually creative form of software development, with 
lots of conceptual discussions as well as programming tasks. It was 
particularly challenging to design a Minimum Viable Product that would be 
able to evolve as users learned this space, since we didn't know _how_ 
things would evolve.

We took inspiration from popular scientific Python packages such as NumPy: 
were possible we avoided adding functions for specific purposes and instead 
built flexible base architecture that people could use to solve a 
variety of problems. This included providing [many examples in the 
documentation](https://scitools-iris.readthedocs.io/en/latest/further_topics/ugrid/operations.html)
to give users confidence.

As some of the first people to consider these problems, it often fell to us 
to explain the core concepts of unstructured meshes to our user base. This 
ranged from being present in office discussions, to giving presentations, to 
[writing documentation](https://scitools-iris.readthedocs.io/en/latest/further_topics/ugrid/data_model.html).

We delivered our MVP in 2022, and immediately saw user engagement with what we
were offering. Giving early adopters something to work with allowed much more
constructive planning of next steps, thanks to the quality of feedback.

#### Performance benchmarking

Unstructured meshes require much larger files for a given data resolution, 
so benchmarking and profiling performance became a key concern. I was tasked 
with introducing performance into our 'test harness', which turned out to be 
highly nuanced and technical task.

* What performance do you want to compare?
  * Different commits?
  * Different dependency versions?
  * Different solutions to a problem?
* How to keep consistent conditions?
* Should dependencies change when benchmarking different commits? (Since older 
  commits were written with different dependency versions)
* How to distinguish noise vs signal when looking for performance changes?
* ...

We found a few existing packages designed to simplify the issue, although 
they each came with their own opinionated answers to the above questions. 
Once we chose Airspeed Velocity, a significant challenge was devising 
customisation suitable to what we needed to achieve.

By being the designated team member for benchmarking, I was able to keep the 
various difficulties in mind and to make sure everyone else in the team 
understood the problems and our solutions. Having performance as part of 
Iris ([see here](https://scitools-iris.readthedocs.io/en/latest/developers_guide/contributing_benchmarks.html))
has improved our understanding of the transition to unstructured meshes, as 
well as catching various changes in performance we would otherwise have missed.

#### Part of an international community

Working in open source introduced me an exciting community. Often multiple teams
around the world will be trying different solutions to the same problem; users
are free to pick the best tool from those available, and development teams can
collaborate to provide solutions that they couldn't individually. The best
packages in this ecosystem are as interoperable with other packages as possible,
and are written as 'thin wrappers' that serve as specialised uses of other '
lower' packages. Attending conferences revealed a clear trade-off between fully
open source projects - enormous development teams that are harder to
coordinate - and projects driven mainly by single institutions - better 
planned/directed development, but slower too.

#### Python's unique strengths

I'm particularly impressed with the pragmatism around Python - the priority is
ease of understanding, and this has grown an enormous community of contributors.
This powerful community has produced libraries which enable writing in Python
while passing execution to more difficult language(s) where better performance
is available (e.g. [NumPy](https://numpy.org/) calling C). The best of both
worlds! These merits have allowed Python to enter 'mainstream' use in  
scientific analysis, where unusually those writing scripts have limited resource
to spare on becoming language experts. Here Python's ease of learning and
debugging shine in contrast to the original mainstream language: R.

#### Coordinating team software development

In 2022 I became our team's 'primary lead' for 
[Iris](https://github.com/SciTools/iris) - our main software library. I am
responsible for:

- Responsiveness - ensuring we catch bugs and help out user questions while
  they are new - minimising impact and maximising value.
  E.g. [our work adapting to the unexpected NetCDF changes that reduced thread safety](https://github.com/SciTools/iris/issues/5016).
- Prioritisation - with more potential work than available time, what should
  be worked on next?
  [See the 'Dragon Taming' project](https://github.com/orgs/SciTools/projects/19?pane=info).
- Philosophy - directing what we want Iris to be, and how we think it should
  work, thus giving a consistent message to users and developers.
  E.g. [how should Iris handle malformed files?](https://github.com/SciTools/iris/issues/5165)
- Horizon scanning - what Iris' place will be within the evolving Scientific
  Python ecosystem.
  E.g. [awareness of how Iris and Xarray compare/relate](https://github.com/SciTools/iris/pull/5025).
- Longevity - establish team cultures that help Iris 'self-manage' long term.
  E.g. [regularly discussing outstanding work](https://github.com/orgs/SciTools/projects/13?pane=info).

Since this is not a 'dictatorship' model, much of the above is about
**enabling** the team to be mindful of these concerns and make decisions, 
perhaps by highlighting something during a discussion, or convening a workshop.
I am essentially Iris' 'advocate' within the team.
