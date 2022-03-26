MFA

https://auth0.com/docs/secure/multi-factor-authentication/step-up-authentication/configure-step-up-authentication-for-web-apps

next
- test out action, explore what data is available post-login action. use acr_value to modify app_metadata. 
	- https://dev.to/kleeut/mfa-with-auth0-actions-updated-code-41p3.
	- https://auth0.com/docs/customize/actions/triggers/post-login/event-object
		- can probably use the acr_values to enable mfa on user when this mfa is triggered from frontend.
- figure out how we determine liveMode as soon as app is initialized. should we use the legacy brand_flags? should we migrate to launch darkly? what is the long term way for managing this?

findings:
- (amr login state is only present on the specific id token used to login. refreshed id tokens will not have that claim.)
- expose `mfa_disabled` value to frontend via rule / action.
- on login, can check to see if mfa_disabled val exists and user is live. if so, prompt mfa screen with acr_value.
- have rule that checks for `acr_value` existance. if exists, modify mfa_disabled value and trigger mfa.
- other ideas:
	- can a rule check to see amr value and update the app_metadata accordingly? or via a hook?
