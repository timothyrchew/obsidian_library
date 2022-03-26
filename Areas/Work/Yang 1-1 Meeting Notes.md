Yang notes

Discuss next:
- Feedback for how to manage the business search discussions. Feels like its been all over the place. Lots of uncertainties and negotiation. From her pespective any feedback about what I could do better? At the RFC level or in navigating details now with Tory?

<hr>

Yang feedback:
- tech debt items on her mind:
	- update the ci process to not re-build the image every time. just run tests against local or agains the existing image
	- migrate PD alerts from sentry to DD metrics. we are currently only capturing exception in sentry, alerts are too noisey
- feedback for me:
	- we mentioned we should break projects into smaller pieces. the self-signup is getting too big. what are smaller pieces we can get started on now or async while the rest is still ironed out
		- action: she said i should communicate this directly to the product team, that she agrees with this sentiment.
	- she would like to bring me into more conversations async. she thinks the 1:1 is really valuable and she would like to be communicating more throughout the week
		- action item: be more proactive about communicationg thoughts, ideas to her.
	- she would like to get me into more broader coversations with more people. i mentioned owning a project. she said she would like to have me own the self-signup, manage the lohika guys and then to take this over. lest see what the PRD looks like and then revist with yang about taking this over
		- action: be preped to lead this. get more familiar with the PRD now to understand and being wrapping my mind around this. be prepped to ask good questions when we do the project kickoff.
	- she said she wants to here more from me about how she can support me. 
		- action: be more proactive about steering where i want to grow, how i want to contribute. share this with yang.


<hr>

Yang Nov 3:
- The full self singup user journey dashboard is up and connected. Full stack tracing is the last piece. The backend update allowing the DD tracing headers is shipped to prod, so I'm working on enbaling the tracing in the sandbox and testing that out.

Qs:
- Is your sense that we will have the Lohika guys for the full 45 days that Ted mentioned last week? It feels like this will diproportionately affect the backend velocity, might increase the benefit of moving to something like Node js. Ted had mentioned we won't backfill, does this include QA? Or will the QA fall on our plates?

My future:
- I'd like to work my way into a engineering leadership role, expanding my contributions not just from a technical standpoint but also from organizational, cultural standpoint.
- In the nearterm my goal is going to be to work really hard to earn a promotion to a senior level, and I when its appropriate I would love to define what types of contributions or how my current role could set my up for that type of promotion. I'd love the chance to own a feature and to demonstrate my ability to work with people to get a project across the line.
- In the meantime, I really value honest feedback about things that I can be doing better, and ways that I can be improving and stepping up to that level.

--

Discussion points:
- obviously my priority these next weeks is going to be on getting established, getting to know the team, laying the groundwork to start contributing. but in addition, my goal is to really watch and listen, learn about the team, and then figure out where I can add the most value based on the strengths and needs of the team.
	- but i would be curious to understand your vision of what it looks like to be successful in this role? how i can be most effective, and best contribute to the overall success of the team?
	- this is very open ended, and certainly will be an ongoing discussion but I did want to see if you had any initial thoughts at this stage?
- where are gaps on the team right now? where is the team most stretched?


Questions Oct 6:
- what is the philosophy around in house vs Lohika. are the lohika folks all full time? who manages them?
- what are the testing / QA aims between automated tests and manual QA. does QA write and automated tests are is there approach always hands on?
- getting back to the issue ted raised about us helping to triage issues in the bond-is channel. has there been any training or communication across the team about how to bring issues to engineers, or is it just ad hoc? way for people to submit a request, knowing that it will be seen by others. one thign that worked well at my last job was we had a channel for all tech issues. it was easy to keep an eye on a single place, add alerts to that channel, and then triage. gets messy when issues get mixed with general chatter.
- fairly open ended question- but im curious to get your perspective and what the biggest needs are on the team right now, where are the gaps? as i get settled in an up to speed, want to focus where i can be most effective.
	- async interactions with product seem to take a while at times



Yang
- my update:
	- things are going well, between writing tests /  typescript refactoring / the first small feature, feeling like i have my bearings in the frontend codebase.
	- definitely will be excited to jump into some feature work when thats ready. but in the meantime, i just want to check in and see if there is anything else i should be focused on other than TS right now.
- Understanding product / eng handoff feature lifecycle
	- do we have a internal published product roadmap?
	- ive noticed we have a couple features that have RFCs or PRDs in progress. I've noticed that we do bring visibility to the PRD that are in progress, and we will sometimes slate them for a spint. I'm curious about how you guage or estimate when that work will get done and when it will be ready for dev? Is that process always synchronous, or are the time when we will actually get started on a feature while the PRD is still being finalized?
- You had mentioned wanting to track velocity via story points
	- Im curious on a broader level about your philosophy around assessing trajectory? is this something that helps you guage how much feature work we can take on, or is this more about about guaging our internal progress and output.
	- do we have success metrics as a brand experience team?
- what can i be doing to most help the team right now?

Yang
- added PG logging this morning, we have two monitors setup to handle the FE/BE errors associated with the Self Signup Dashbaord. The thresholds are really aggresive right now, and it set to just notify me. Once we go live, I spend a bit of time tweaking these. What kind of PG escalatino policy would we want to appyl here?
- I'm setting up a custom Auth0 rule that will append a value in the token indicating if a user is new. From there I will get RUM setup in BondOS, we can embed thsoe metrics into our Self Signup dashboard and we will have a single view of the full user journey.
- Does that solve what you were hoping to do here? Anything else to consider here?





General questions:
- Trajectory / speed
- process vs enabling us to build quickly. have we been burned by this? how do they find the right balance?\
- philosphy around more fullstack vs fe/ backend engineers. 