---
layout: post
title: Maintaining a record of personal contributions
tag: information management
---
 
## Ways to keep a record of one's personal efforts and contributions. ##
 
### John C. Nash ###
 
#### 2017-6-21 ####

In this day and age of often temporary employment, people are told to "keep their CV up to date".
This is a chore, but it is not difficult to keep a list of activities. What is more challenging
is to maintain verifiable records of that activity. Here the two key words are "maintain" and
"verifiable". This discussion suggests some ideas for doing this, as well as a possible 
service and revenue opportunity for appropriate organizations to assist.

### Motivations ###

One of the concerns for those of us who write, design, paint, sculpt, or otherwise have working
lives where we create or build things is to keep a record of those activities. While the 
Internet has allowed much greater access to a number of repositories of records, it is a sad
reality that many libraries and archives have been starved of resources. Items that were 
previously available are now inaccessible or, worse, lost forever.

In my own case, I have an academic and scholarly career of over half a century. Even journals
in which I originally published are awkward to access, and items like technical reports or
magazine articles are not available even to those in reference libraries. 

Where this lack of access to sources becomes problematic is when someone claims authorship or
participation in some activity, and it is important or useful to verify their claim. Inflation
of CVs is widespread. We have only to note how "Harjit Sajjan apologizes for claim about Afghan offensive"
(Toronto Star, April 29, 2017) after Canada's Defence Minister overstated his role in a military
operation. Twenty years ago, one of my former colleagues who became Deputy Minister of Health
for Alberta became the center of an independent review into her resume commissioned by the government.
This review reported the resume 'overstated some of her qualifications, but that her "basic credentials 
were sound." '

Public uproar about claims on CVs point to a more general stretching of the truth. In a competitive
world, this is hardly surprising. On the other hand, we want -- perhaps need -- to know who programmed 
key software and who simply went for the coffee. Or who wielded the scalpel and who only pushed the
gurney into the operating theatre.

For those who are truly capable and productive, there is a benefit to having a verifiable record.
In my own life, I have encountered people who looked at me askance when I mentioned one or two of
my books, only losing their mask in incredulity when the objects were placed in front of them. One
can only speculate how a CV is received absent the evidence. 

Assuming the person presenting a CV is indeed the author of the claimed activity, there are benefits
to that person and to the recipients of the document if it can be certified correct in some way.


### Example mechanism ###

For myself, given that I am a chronic and addicted writer, programmer and tinkerer with ideas, it
is important to me to have a record, if only for myself. To make the record at least moderately
credible, I have taken to maintaining machine-readable copies, scans, photos or other records
of my work. Moreover, I maintain a bibliographic database of each piece of work I consider more
or less complete. Over time, some of these are converted to more formal outputs, and the database
is (hopefully! -- I maintain this alone) updated.

A summary of the technical foundation of this record keeping is as follows:

- I keep the bibliographic database, which includes records of some designs and similar "objects", 
using the BibTEX format. (?? ref.). Many workers use this structure, which is widely supported
by a number of professional organizations and scientific journals. BibTEX is part of the larger
TEX and LaTEX text processing system for mathematics and science. BibTEX allows for notes and
many other extra pieces of information.
- I maintain a collection of scans, photos and similar machine-readable documents in a set of
subdirectories by year under the main **jnpubs** directory. 
- To give context to my work, I have a template curriculum vitae file in LaTEX with some extra
encoding.
- A computer program (written in Perl, now becoming dated) generates two LaTEX files of a 
lifetime and a past-7-years resume and processes this. Counts are provided of different
categories of work. I caution that categorization is an activity much loved of administrators,
but deciding if an article commissioned to appear in a major journal is "refereed" or not
could occupy the courts for many months. I prefer simply to report the items where they were
published. Since I have versions of the work available on request, people can judge the merit
for themselves.
- The entire **jnpubs** directory is under version control using the **subversion** software,
using a repository on a personal virtual machine at the University of Ottawa. This serves not
only for version control but for off-site backup.


### Authentication, audit and evaluation ###

There are some key elements that are lacking in my system. 

First, I have not provided a mechanism to prove that the works I claim were actually done by
me, nor that any web-site materials are really mine, and not co-opted work by someone else.
Note that there are digital signature systems that could be used. That is, I could digitally
sign the works with my own private key. However, some key authorization service would be needed
to establish that the key was trustworthy.

Second, while I can provide copies or scans of articles etc., there is no independent verification
that the claimed sources are correct. The journal volumes, numbers and pages have not been checked
by someone other than me.

Third, as I have indicated above, there is no measure of value placed on each item, even to indicate
the volume of work involved. This is not something the author can properly provide. Every mother 
sees a genius in her child.

Here we recognize a role for third parties, namely,

- to provide some level of authentication of the claimant as author of the work;
- to verify that the work is correctly identified and referenced; and
- possibly to provide some measure of quality or value of the work.


### Revenue opportunities ###

The three roles for third parties are opportunities for revenue-generating activity. Since,
however, these activities involve trust at various levels, they are particularly opportunities
for organizations that already have a standing in society and which already engage in some form
of accreditation or evaluation. Here universities, colleges and professional organizations would
appear to have a natural standing.

The components of any authentication, audit and evaluation service that could generate revenue
are as follows:

- setup of a repository of work for an individual using some standardized technology;
- ongoing maintenance and hosting of the repository;
- authentication of the individual and his/her works, possibly using some sort of digital
signature scheme, either of the individual or the organization doing the authentication;
- verification of works in the individual's repository according to some established process.
It is doubtful if every piece of activity can be fully verified, so a procedure with checklist
approach is likely to be needed, with this process clearly stated.
- supply of verified and authenticated CV to recipients who the individual has nominated;
- possible review and evaluation of part or all of an individual's work.

This last possibility suggests the potential establishment of some sort of qualification by
portfolio. However, reviewing and evaluating work is much more demanding than simply 
verifying that the records exist and match the claim of authorship. We now have to decide
if there is merit in the work. Nonetheless, it could be attractive to applicants to have
their work reviewed and evaluated and to organizations to do such reviews if the process
were sufficiently well-defined and the costs and rewards appropriately balanced.

