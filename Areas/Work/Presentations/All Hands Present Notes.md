# Prep

improvements
- log dd views - setup funnel analysis for signup
- improve dashboard
	- more nuanced error tracking. connect to rum error tracking rather than catching all errors.
	- add non form errors
	- add things like session length, geography, browser / devices
- showcase the session dashboard overview



# Outline

Goals:
- Walk through some updates that we are making to both monitoring and alerts
	- The demo is going to be focused moreso on frontend BondOS codebase, but the broader iniative is one that we will be applying to both the frontend and backend.

Why:
- In support of moving towards more continuous deployment. both active and passive aspects.
	- we want better visibility in how our app / features are functioning
		- one click way to via a dashboard to see if there are problemic issues
	- we want to be confident in our alerting
		- two fold for us:
			- our current alerting is very noisey. if there is a real issue, its easy for it to get lost in the noise.
			- bondos apps have multiple teams - we want errors routed to the right team to handle them. we've talked about having clearer ownership and accountability as we scale up. we want this to be clearly reflected in the way that monitoring and alerts are set up.

How:
- We are replace Sentry with Dadadog RUM (real user monitoring) tool.
	- Gives us more tools to build unified dashboards
	- More configuration around monitors
	- we gain a couple really helpful user insight / debugging tools.
- We are implementing error boundaries to tag errors to direct them to specific teams.

What:
- High level RUM App dashboard
- Feature or component based dashboard: Closer look to a dashboard we built for the self signup flow (another example would be the CSR portal)
	- targetted visibility into  the usage, potential issues.
		- performance metrics
		- error tracking
		- user insights 
			- some at the request of product
			- also datapoints for us to consider (validate assumption, guide our priorities)
		- key dashboard showcases
			- user behavior funnel
			- user session
			- specific user behavior
			- unified insights into multiple services, in this case two frontends and one backend.
	- can dig into specific user sections
		- datadog rum sessions - one new feature i want to highlight
			- (shoutout to nick: this is most definitely not a copy of their main offering)
			- can pick up an error, can drill down to see the real time user interactions leading up to the event
				- i can start with any error here, and look up the session associated with it
					- can see the moments
					- i can look up the distributed trace here. this will include all os-backend work.
				- the use can go beyond debugging. i can look up a users very first session logging into the app. this can also be useful for looking at user behavior aspect to this as well.

approach:
- start with the dashboard as the entry point for a specific feature. view this as part of the delivery for new features. unified view of the relevant services that are part of the work across codebases.
	- performance monitoring / alerting
	- feature usage
	- in depth debugging - new tools at our disposal
		- drop into a single session
		- watch the replay, can see the events leading up to a specific error
		- see attribution
- connect monitors here are connected to pager duty
	- triaging based on ownership
	- threshold based via pager duty
	- streaming errors to slack. actively watch, reduce unhelpful errors- ongoing process.



<hr>


Problems we are looking to solve:
- End user visibility and usage metrics
	- multi purpose function
		- rolling up user metrics for non-technical users. its clear that this is going to be a lot more applicable for frontend features. not intended to replace a user beahvior tool or more robust user journey tracking, like we would get through a tool like full story. but
		- can serve as an entry point into errors or user sessions, performance monitoring for a specific feature.
	- RUM and datadog dashboard update
		- unified metrics across codebases
		- fulls track tracing
		- session replays
- Refined montioring and PagerDuty alerting
	- can tie this in to dashboard metrics / monitors
	- thresholds, intelligently grouped errors. removing the boy who cried wolf symptom. less noise. (not an overnight process, but we have more tools here)
		- thresholds vs
	- add more context to errors to enable domains of ownership and pagerduty services reflecting team ownership within a single codebase.
		- error boundaries and error context object


#

(drop links in the chat)

As the BREXP team we've been making some updates to our approach around monitoring and alerts around the BondOS app, and wanted to quickly share some of the changes, as well as some new tooling we have available for debuggin and user inights.

One of the motivations behind this work has been to support of our team moving towrads more continuous deployment. As we reduce some thresholds for deploying and we deploy more frequently, we also want to be shoring up our support structures that
- give us better visibility into how our app / features are functioning
- increase our confidence in our alerting and our ability to rapidly debug issue.

Our strategy for bondOS has been to lean more into Datadog across the full stack, whereas previously we used Sentry as our primarily frontend monitoring tool.

The reason for switch largely hinges on some of the additional capabilities we gain through Datadogs RUM, which is their dedicated frontend monitoring tool. 

To highlight a couple of these new features, I'll start with dashbaords.

Many of you have probably used DD dashboards in the past, and by bringing the frontend under the DD umbrella we can leverage dashboards in this part of the stack as well. I'm a big fan of dashboards as a supplement to monitors or alerts. 
- Whereas alert + pager duty is by nature more passive and reactionary,  dashboard can be really helpful in balancing that out to have a comprehensive approach to maintenence and improvements that can be more forward looking and proactive. It can be easy to develop tunnel vision when just jumping from issue to issue, and dashboards can quick way to periodically step back to assess the healthy of a feature or application at a higher level.

I will start by zooming out to the application level dashboard. This is something that we get for free by adding RUM to any frontend app.

As far as dashboard go, most of this is pretty self explanatory- its a mix of web vitals, user insights, and some high level error tracking. I think this can interesting, there can be some high level takeways when looking at whether there are specific pages, or user actions that are triggering an outstanding amount of errors.

But I will say that the types of dashboard I personally get the most value from are narrower, customer built, feature focused dashboards. 
- This view is something that we built in concert witht he self-signup feature. In this case, this work straddled three codebases, and we wanted a an easy way to aggregate the vital pieces in a way that would just take one click to check on. So particularly as this is launching, this is something that I'm watching more closely.
	- This is likely someones very first impression or app. If we lose someone in this process, or if something goes down here, there is a chance they aren't coming back.
- The intention here is multi-faceted: 
	- there is a narrower performance tracking, error monitoring purpose here
	- But there is also part of this that gets at the "why" behind our feature work.
		- its a way for us track usage, to test our assumptions and to even introduce OKRs around our feature work or deliverables. 
		- I think some of these objective metrics, if harnessed well, should help augment our decision making process moving forward, both from an engineering and product standpoint.
- Whereas it can be easy to ship and move on to the next feature. This is a pretty accessible way to make a one stop place to periodically check back on a feature and to encourage that sense of ownership for things that we've built and shipped.
	
I mentioned that we have some new debugging tools. One of my favorite parts about RUM, is that it introduces the concept of tracked user session. For those of you that have used frontend user insight tools in the past- you'll be familiar with user session replays. Shoutout to Nick (and his time at full story), DD is 100 percent riding on the coattails of what FullStory and others are doing.

RUM sessions are a collection of consecutive user interactions with our application collected while they have the tab open on our app. And the cool part is that these can be played back in real time to give us some really valuable insights into errors or user behavior.

So to connect session back to dashboards, one of the main practical uses of a dashboard like this for me, is to use this to get really targetted user session details.

For example I can pull up a cluster of errors and I can watch the session where these happened. I can look at this funnel, and I can try to glean insights about what might have happened to cause 79 percent dropoff at this second step.

To quickly show what this looks like, I'll grab this cluster of errors. I'll click to view RUM events. I'll see all the errors that happened in that timeframe. And I can click into to replay that session.

In this view we have a replay of the user's session, as well as a sequence of the actions and errors that occurued during the session.

(There are a ton of applications for this, and IMO its really tool when it comes to debuggin an error or understanding how users are interacting with a specific part of the app. I can say already, with a very small sample size- this has helped us uncover an edgecase that we hadn't fully anciticpated when this was launched.)

We can jump to the timestamp associated with any of these actions. Here they clicked on the SET PASSWORD button and then got an error.
- looking at the contents of this error, they were actually trying to set the password without having gone through the first step of submitting their user info.
- So this uncovered an edge case in this flow that we had not protected against. And we were able to patch that.
- This is a case where seeing this error, without the context would ahve been much harder to figure out what went wrong here.

One other cool thing I will highlight is the full strack tracing we have. Another benefit of bringing this all under the DD umbrella is that we get distributed tracing for these actions. DD adds some custom headers taht get passed as part of the request and we can connect this across services. I can view this trace with APM if I want to dig even deepr.

I do want to touch on the privacy / PII concerns around this. It understandably can feel quite invasive. But we have taken steps to ensure that not PII will ever be visible within these sessions. In the self-signup flow all user inputs are masked. And then we've gone a step further within bondOS. its on the highest privacy setting there, so all text is masked. 
- You can see UI elements, you can see icons, you can see the cursor. But there is no legible text anywhere.

(do i mention user id?)



The last thing I want to cover is the reworking of our alerts. This has been, and will continue to be an incremental approach.
- A couple painpoints with the current approach, which has been to handle these through Sentry.  
	- We have been a struggle with very noisy alerts. It felt a lot like the boy cried world with a very low pwercentage of alerts being actionable. So we are exploring threshold based monitors, that we are hoping will cut down on more of the noise, and will alert us about an urgent issue. DD gives us some more levers to pull in terms fine tuning these, but it will take some experimentation to find the sweet spot. And up until that point, we will just run this alongside Sentry without making changes to PD until we are fully confident here.

The second change here structuring our alerting in a way that better reflects the multi-team owership that exists within the bondos ecosystem. We had a version of this that existed via some url path matching in Sentry, but we've shifted to code level approach that uses Error Boundary to tag errors by team ownership.
- Under this model, thinking of React as a tree structure. Errors will buble up through the tree until they are caught by a boundary and sent to one of our monitors. 
	- This lets us be a little more nuanced with how we define onwership, especially when it doesn't always align 1:1 with the url path. 
	- Making the error handling / tagging part of the code review process helps build the habit of thinking about how this is going to be monitored and makes that more visible to the team. Rather than merging code, and having to remember to go update a URL path in sentry after the fact. 

The last benefit I'll highlight of moving monitors into DD is that, similar to what I showed in the dashboards, if I get a pager duty alert from a frontend DD monitors, I can immediately go lookup the specific user session that triggered that alert and watch the replay. And so that to me is just invaluable in terms of cutting out a lot of the guesswork that goes into trying to reconstrcut the steps that led up to an issue.



<hr>



Be prepared to answer:
- We used Sentry for Error tracking. Aren't we losing the Error tracking piece?
	- I will agree that on the issue of error tracking, Sentry has some unique benefits. However when looking at how we are using Sentry, at least for our team, we are really just using the monitoring. the actual error tracking, resolving is all happening in pager duty and slack. 
		- So in our opinion, some of the benefits we gain from unified our monitoring and alerting withint Datadog really outweighed some of the aspects that Sentry added around error tracking.
			- I will say that at this point, we haven't removed anything Sentry related. We have just worked to get Datadog up to par with Sentry in terms of intergrating it into the frontend and beiging the log coverage up to parity with Sentry
	- the even bigger piece is that we can connect errors to session replays via datadog.
- whats wrong with the path based approach for error triaging? id rather do it from the monitoring platform, i don't want this hardcoded into the code. thats too much work. i want to be able to shift this without having to put in a PR.
	- you still have everything before. you can still utilize paths. this is just augmenting and broadcasting the errors in a second way, without affecting the original message, so to speak. 


Random thoughts to add:
- feature based dashbaord really useful as part of the release plan. when things begin to roll out its a quick way to check on an aggregation of key metrics
	- also can be part of OKRs for the success. helps us validate our assumtpions about how the product is being used. if its meeting a need. helping drive further engineering decisions.
- touch on the different ways to filter down sessions to see very specific areas. show me all the sessions where that triggered an error when this search button was clicked. go back in time and watch in real time. see the full stack trace- including the backend services that were part part of this response or error.
	- if we are debugging a specific users issue, we do have the capability to look up that user by their userid.





