# Next

on test breaking in ci:
- try more things to replicate in using netlify cli locally. until i can replicate a failure locally, its a total crapshoot.
- could coverage exclusions mean anything? scour recent pushes to see if anything recent could have possibly affected it.
- try running "only" this test in ci. could the volume, order, or addition of any other tests have affected it?
- try logging out the targetted dom in CI to see the state of the form
- try using fireEvent instead
- try using await whe fetching the button
- check package version, make sure same locally and in deployed version. any deps that could be different? anything weird from picasso?




account page:
- do MFA section with testsw
	- if live mode, allow exisitng users to reset if they have it using the current backend


--

port over sentry alerts
- clean up the self signup errors to not have false trigger for form errors





--


tickets

Email verification:
-

