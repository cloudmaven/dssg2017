---
title: "Cloud adoption framework"
teaching: 20
exercises: 0
questions:
- "What is our cloud framework?"
- "How does this map to my research?"
objectives:
- "Understand the cloud framework we present"
- "Map your personal perspective and objectives to this course"
keypoints:
- "There is a framework for the cloud that includes { yourself, colleagues, vendors, stacks, and third parties (open/closed) }"
- "The stack is five components { compute, store, manage, web, service }"
- "'Service' is just providing the other four components free of underlying concerns like VMs ('Dinner with no plates and no table')"
---


## cloud101 


This course is is *open*: Publicly available and we have tried to make it self-guided 
as much as possible. If you are working on your own you will need to set up and configure
cloud accounts with up to three vendors/platforms that we discuss: Amazon Web Services, 
Microsoft Azure and Google Cloud Platform. In all cases you can get a small test
account (with credit on the order of a couple hundred dollars) but you may be obliged
to provide a credit card.


> ## Why Are We Here? 
> If cloud computing was just Virtual Machines and Storage we would not be here 
> today! There must more going on; and our first evidence is the appeal of this
> course across research domains. The first class included 45 attendees with 
> representatives from **Oceanography, Libraries, Biology, eScience, Forestry, 
> Bioinformatics, Sociology, Computer Science, Hospitals, Environmental Science, 
> Astronomy, Electrical Engineering, the Information School and King County
> Metro!**
{: .callout}


![red queen](/cloud101_intro/fig/redqueen.png)


We work out of the eScience Institute and UW IT to try and accelerate research by helping
the **Researcher**.  Our model for the **Researcher** is the Red Queen from 
_Through The Looking Glass_: A person running very fast just to stay in once place. 
We tend to work with Researchers who can make the time to stop running for a moment 
in order to learn about and evaluate the cloud as a research computing platform. 
This one-day course is primarily introductory but also as hands-on as we can make it. 


> ## Our Call To Action To You
> Our call to action to you: Share awareness of both this course and of the eScience 
> Institute with your colleagues. 
{: .callout}


### Your way foreward after today


- Bookmark [cloud maven](http://cloudmaven.org)
- Bookmark [this course](https://cloudmaven.github.io/documentation/rc_cloud101_immersion.html) 
- Share this course with colleagues
- Visit the Data Science Studio: Consulting and vendor office hours 
- Use: Search engines, YouTube, StackOverflow, UW Help to get more assistance
- Know about, use: GitHub, Jupyter, Django, Slack
- Apply for research computing credits on the platform of your choice
- Set up your cloud account, build your environment, get back to your research


### What this course is 

- A description of how you choose a cloud platform
- Elements of implementing your research on the cloud
- Overview of security, cost, account management, processing power and time value
- Hands-on: Start an instance, store some data **Across Three Cloud Platforms**
- Hands-on: Build a Django web application with an API
- Hands-on: Build and operate a compute cluster
- Enthusiasim: Researcher advocacy and the eScience Institute


> ## Challenge (click arrow to the right to open)
>
>  - What is the Google technology called 'TensorFlow' about?
>  - What is the corresponding Microsoft Azure technology?
>  - For AWS?
>
>
>  - Link for [Google Tensor Flow](https://www.tensorflow.org/).
>  - Link for [Microsoft Azure ML Studio](https://studio.azureml.net/)
>  - Link for [AWS ML](https://aws.amazon.com/machine-learning/)
{: .challenge}


### What it isn't


- A comprehensive overview of managing a public cloud account 
- A license to jump onto the public cloud and start creating massive compute jobs
- An advertisement for some particular cloud vendor 
- Extensive comparison with **BAM** or UW HPC computing 


### Two severe weather advisories


> ## The burden of cloud management is on each of us
> There is considerable detail to learn about managing your work on the public cloud.
> Without this skill life can quickly become expensive; for example if you accidentally 
> allocate > expensive resources and leave them runningi. 
> (Cloud instances can be turned off without losing state/progress and they can
> be saved as memory images.)
{: .callout}


> ## Lemons from lemonade
> A good way of getting some bad news is to publish **and then delete** your cloud 
> access credentials on GitHub. GitHub supports versioning: Someone who is not your 
> friend can roll back your public repository to the version where the key was 
> present, grab that key, and start using your cloud account at your expense.  
{: .callout}


### Our Public Cloud Framework


This *framework* is the vocabulary and relationships we use to describe using the public
cloud platform for data-driven research. Here comes the jargon storm! In what follows we
assume you are a **Researcher** focused on data-driven science and that you are interested
in adopting the cloud as a way of streamlining that process in some capacity.


As a Researcher you do perfunctory processing and exploratory processing. The cloud can help 
you with both, but at a cost: The time you invest to learn new methods. We call this 
*cloud adoption* and the core premise is that you no longer have a *familiar computer*
with an attached storage system where you log in and do your work. The cloud model is
(they like to say) *cattle not pets*: You have a huge pool of available compute resources
and you rent them by the hour. When you are done with them you simply *Stop* or *Terminate*
them and they go back into the resource pool.  Before continuing let's do a quick cost 
analysis of what this means: How does a cloud machine compare to a desktop?



> ## A Cloud Under Your Desk
> A good desktop might cost $3000; and a very powerful cloud instance will cost about
> $0.40 (USD) per hour. Let's say for the sake of argument that they are equivalent
> in compute power and attached storage. If you work eight-hour days with four
> weeks of vacation then your annual compute cost is roughly $800. Over three years
> your "cloud under the desk" runs you $2400; but you can make this cheaper if you do 
> not need the compute power; or you can throttle it up when you need a lot. You might 
> also ask: What are the additional tradeoffs and other factors? Rather than try to
> spell these out we refer you to the [cloudmaven](http://cloudmaven.org) website 
> and to our office hours for consulting: The subject is too complex to address here;
> but to first order renting a cloud machine and turning it off every night can be 
> quite cost-effective.  One thing we will mention is that when you come in in the morning
> and start your cloud machine you will have a couple minutes to go get some coffee. 
{: .callout}


More generally: Cloud adoption is *awareness* + *learning* + *matching* 
the technology to the details of your research.  If you consider the pie chart of 
your time sliced into activities: Our goal is to reduce the slices that are detracting
from your *write and publish papers* slice.  We can also introduce you to new things
like publishing your data and your software; cf the *executable paper*.


#### The cloud components are compute, store, manage, web and services


  - Compute = VM = 'instance' = a computer = AWS EC2 = Azure VM = Google Compute Engine


    - Compute means 'instance + OS + memory + pre-installed tools + ...'
    - Compute as a service example: AWS Lambda
    - Third parties like GitHub provide 98% solutions: The task is finding them


  - Storage = S3 = Blob = Google Cloud Storage (and Google Drive)


    - 0.024 dollars per GB-month (and x 1/2 and x 1/4 for archival applications)


  - Manage = Databases... SQL, Not Only SQL... Data Warehouses... query machinery


  - Web = Web services, web sites, APIs, Clients, confederation, ...


  - Services = All of the above simplified: Often no Compute involved


#### The cloud facets to learn about are admin, cost, security, scale and time


- Admin is easy to dismiss; but it is always present, even on the cloud


  - When we reconstitute an AWS AMI we habitually run this command:


```
% sudo yum update
```


  - You can keep your cloud environment up-to-date without becoming a sysadmin


- Cost is 'one penny per processor-hour and three pennies per GB-month'
  - ...but there are more details...
  - ...and there are cost calculators... which can be misleading...
  - ...so we recommend these steps, for example for grant writing
    - Do a Back Of The Envelope (use cloudmaven.org or our office hours)
    - Start with a budget total target and work backwards 
    - Use calculators but be prepared to be skeptical of your results
    - Pad your estimate because things may go wrong
    - Remember that the cloud does not 


- The cloud is secure provided you manage your credentials
    - keep them out of GitHub (which can be rolled back)
    - Use MFA
    - Follow other standard procedures
    - If you are going over to PHI / HIPAA there is a whole 'nother level to this


- The cloud is cost-competitive with traditional **buy and maintain** (**BAM**)
  - This in part depends on your percent usage: Machine time / wall clock time
    - Studies indicate the breakeven is currently around 50%
  - The cloud *crushes* **BAM** on your wall clock time
    - Start wait is zero


### Should I move to the cloud?


This really depends on the value of your time in relation to your research budget; and on how 
much of your wall clock time you spend doing computing.  It also depends on your team's capacity 
to assess and learn cloud tech for your work. This might be very fast - which is what we find in
the majority of cases - but if you are getting into sophisticated work e.g. using a web framework
then there could be a substantial bootstrapping effort required. 


  - You are the protagonist in the play
  - The other actors are funding agencies, cloud vendors, third party providers
  - There is your domain-specific community 
  - There is the open source development community. They may have built 98% of what you need.


Our call to action is: 'Learn enough to learn enough to learn enough... to evaluate cloud migration.'
The simplest approach is to visit us by appointment or during drop-in office hours. 


### Which Cloud Should I Use?


"It depends..." and here are some factors.


- In your field of study: Are certain cloud providers in favor? For compelling reasons? 
- What sort of compute power are you looking for? Single machines? cluster? GPU? FPGA? HTC? HPC? 
- Cost: Per machine, en masse, Spot (bidding) market, reserved VMs, dedicated VMs for security, ...
- Compare your compute tasks with case studies at [cloudmaven.org](http://cloudmaven.org)
- Be aware of credit programs: Azure and AWS; Google may follow soon...
- Be aware of cost tracking and billing as a way to validate your cost estimates
- Development tools for the different clouds
- What services do the vendors provide that may make life much simpler?  
- Docker: Your first key to migrating from one cloud to another


|Category|AWS|Azure|Google|
|---------|:--------:|-----:|
|vCPU hour|$0.01|$0.01|$0.01|
|storage GB-month|0.024|0.030|0.030|
|5900 vCPUs x 53 hours|3400|unclear|$4000|
|DBaaS per month|what|evil|lurks|
|Egress < 15% of MB|0|0|unknown|
|attached storage GB-month|0.10|unknown|unknown|


## Questions For Discussion


- "What do we mean by 'Data Intensive Science'?"
- "What are Web Apps, Clients and Servers?"
- "What is the purpose of the Django web framework?"
- "What is Elastic Beanstalk? What are its analogs on other cloud platforms?"
- "What is cluster computing?"
- "After this course am I ready to jump into the cloud?"
- "What is the post-course strategy for filling in blanks and building cloud skills?"


*This project is funded in part by NSF Campus Cyberinfrastructure - Infrastructure, 
Innovation and Engineering Program (CC*IIE) Grant Aware No. ACI-1440281; Principal
Investigator Brad Greer, CTO, UW-IT.*

