Action steps:
- we agree about wrapping errors for a specific team
- koty wants to drastically reduce PD alerts. 


--

Quick update on what Yang and I have discussed around alerts. 
- as you guys know, the plan is to retire Sentry for bondos and unify monitoring and alerting under the datadog umbrella. among a number of reasons, one primary one is that we have more at our disposal when it comes to configuring our monitors and lets us move to more metrics based alerts. 
	- as we are all aware, we are currently pretty indescriminate in what gets forwarded to PD
- a second change we want to make here is that we want to route alerts to the specific team with ownership over that part of the app. 
	- for this second point, there are a couple different approaches we could use here. and i would like to get your thoughts here. but first, i think the biggest issue is for us to think about is how we work surface more urgent / actionalbe issues, how we surpress noise


more effective alerts:
- visibility into all issues via a slack channel like we have currently. ensure we are looking at these. we should be working to resolve these and removing them if they can be fixed.
- ideally we are more judicious about what triggers a PD alert. we've acknowledged that there is currently a lot of noise, and the alerts are usually not actionable.
	- approaches
		- my thought is that we have some mix of thresholds with more aggresive alerting on what we would identify as urgent or more serious errors. 
			- will require some fine tuning and experimentation
			- what would we use for thesholds?
			- what types of errors that we think should always trigger an alert?


routing errors to the right team:
- we can use error boundaries around specific area of responsibility. catch that error, tag it, and then send it to that team.
	- right now this really just concners the treasury team.
- we could filter by app url attached to the error on the datadog side.
	- the downside to this is that our catch all side would involve a lot of exclusions and would require more maintenence


--

Depricate:

for routing errors to the right team:
- we can use error boundaries around specific area of responsibility, catch that error, and tag it that team.
	- problems
		- we need to rely on global error capture for full coverage. error boundary won't catch it all.
		- errors tend to get logged twice- at the boundary and at the top level. so we can route custom errors to the right team, but we will likely still see the errors appearing at the global level meaning noise for that monitor. 
			- is there a way that the global error can ignore the errors that get caught by lower boundary? seems unlikely but worth a look.
- we can use url based routing. right now any part of the app that the treasury team would own has its own path. this is captured in the errors by default, and we could route by this value. 
	- do avoid duplication at the top level brexp monitor we would need to have a somewhat complex set of rules to exclude all the paths that are owned by other teams. not very scalalbe and easy for that to fall out of sync.