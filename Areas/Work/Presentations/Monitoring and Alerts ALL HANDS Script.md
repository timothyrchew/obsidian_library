# INTRO

Thanks ted!

The main motivation behind our work to harness Dashboards and update Alerts has been to support our move towards more continuous deployment. As we reduce some thresholds for deploying and we deploy more frequently, this piece is about investing in our support structures with the goal of better visibility and more confidence in our alerting.

Our strategy for bondOS has been to lean more into Datadog across the full stack, whereas previously we used Sentry for frontend monitoring.

So I will walk through a couple beneifts from this switch, largely hinging on our use of Datadog dashboards and recorded user sessions.

# WHY

So regarding dashboards... i think a valid question might come to mind: "why use dashboards when we have robust monitors set up".
The way I think about this is that... alerts + pager duty is by nature more reactionary, and I see dashboards as a way to balance that out through an approach that can be a bit more proactive in addressing problems before they snowball into larger issues, especially when shipping new features.

# DASHBOARDS

So...to get into a real world use of a dashboard. We do have a high level dashboard for the entire os frontend app, but I will dig more into a custom built view we built in support of the self-signup feature. In this case, this work straddled three codebases, and we wanted a an easy way to aggregate the decoupled vital pieces in a way that through one click we could get a sense of the health of this feature. This is something that we have been, and will be watching much more closely as a feature is live and is still new. And then certainly as time goes on we can really shift more towareds a reliance on monitors.

So from this view, youll see on the top left that we have a session count. One of the great things about DD's frontend tool, is that we have the concept of tracked user session. A session is a record of user interactions with our application collected while they have the tab open. And then we have the ability to play that back in real time.

One of the most practical uses of this type of dashboard for me, is to start at a birds eye view, and then to drill down into very specific types of user sessions.

For example I can pull up a cluster of errors and view the sessions where these occured. I can look at session that dropped off at step 2, to see if we can identify any friction point in this signup flow. I can go back an watch a user's first session after logining into bondOS for the first time.

So this really can serve a broad range of purposes from debugging, all the way to product insight and user behavior.


# SESSION DEMO

To quickly show what this looks like, I'll grab this cluster of errors. I'll click to view RUM events. I'll see all the errors that happened in that timeframe. And from here I can click into to the session replay for that error.

And now I get to watch a replay of the user's session, as well as a sequence of the actions that let up to this these errors.

We can jump to the timestamp associated with any of these actions. Here they clicked on the SET PASSWORD button and then got an error.
- at look at this single action and see a bunch of metadata, as well as the full stack trace associated with this action all the way down to os-backend responding to this request. 

So from this view, we can retrace the steps of a user up until the point where the error was thrown. 

This specific session actually helped us uncover an edge case in the signup flow, where this user loaded the page directly into the second step. And it was failing when they were trying to set the password without any of the user info from the first step.
- Had we just had this error in isolation, it would have been much harder to figure out what went wrong here. In this case we were able to see this and get a fix out quickly.
- Thankfully this story does have a happy ending, and the user goes back to step 1, fills out the full form, and then is able to successfully create their new account.

# PRIVACY

I do want to touch on the privacy / PII concerns around this. This can feel quite invasive. But we have taken steps to ensure that no PII will ever be visible within these sessions. In the self-signup flow all user inputs are masked. And then we've gone a step further within bondOS. its on the highest privacy setting, where all text in the app will be masked. So there is no legible text in those session replatys. 

# PRODUCT FOCUS

The last thing I'll say about this dashboard, is that it certaintly is about performance tracking, its a tool for debugging issues.
- But there is also an important part of this that gets at the "why" behind our feature work.
	- its a way for us to test our assumptions about how much a feature is being used, our assumptions about the typical user journey.
	- I think some of these metrics can augment our decision making process around what improvements or bug fixes might have highest impact on end users.
- So to maximize the utility here, I think there can be value in building these with input from stakeholders outside of engineering. A couple of these metrics where driven by requests from the product team about things they wanted to track, and so this can be an accesible way to make that available to them.

# ALERTS

The last thing I want to cover is the reworking of our frontend monitors.

Our current monitors live in Sentry, but we are working to move them over to Datadog.

A brief run through the updates:
- We want to make alerts less noisy. Our current alerts feel a bit like the boy who cried wolf. So we are looking at threshold based monitors and aiming for lower volume and hopefully higher actionability. DD gives us some more levers to pull when it comes to fine tuning these. Right now these are running alongside Sentry, and once we feel good about the thesholds we will look at switching over to make these primary.

- The second change has to do with team ownership for frontend issues. You'll see that we have three teams with monitors that pull different parts of the frontend. 
	- we are shifting to code based approach to tagging errors for specific teams, rather than just relying on path matching for routing errors. This is being done with React error boundary components that will allow for more more nuance in places where urls doesn't align 1:1 with team ownership, and we have shared components used in a number of places where one team should be the primary owner.
		
- The 3rd and last reason, is that when it comes to actually responding to alerts, by moving this under the DD umbrella, we will have user session with a replay, attached to every error. Our hope is that having that extra peice of context, can help cut down on the time to get to the root issue and ultimately leads to quicker resolution times.


That is all I have for now. Feel free to reach out, happy to expand on anything.



