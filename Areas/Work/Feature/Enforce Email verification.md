Next:
- add screen to enforce email verified in brand portal
- build out email confirmation screen in self-signup app



os-backend changes when going live with brand portal (services.py L188):
- this should be as simple as changing the default login url at the application level. this will mean that all redirects after an action (email verify, password reset) will go here. redirects from the apps login page should still work normally, since the callback is handled at the app level.
- https://auth0.com/docs/customize/email/email-templates#configuring-the-redirect-to-url
- reset password page should redirect to brand portal
	- auth0 will redirect to the default login page after password reset success. this does not seem dynamic. will likely need to change this at the auth0 application level here (https://auth0.com/docs/authenticate/login/auth0-universal-login/configure-default-login-routes) or here (https://manage.auth0.com/dashboard/us/dev-bond/applications/cNV4Ehb6uJqZR5Egl5R9OZWgKWCGj2jP/settings)
- default verification email should also redirect to brand portal
	- in email template (https://manage.auth0.com/dashboard/us/dev-bond/templates) you can set "redirect to". we can probably hardcode the change here.
	- https://auth0.com/docs/api/management/v2#!/Tickets/post_email_verification
		- overview of the email ticket parameters
			- explaination of result_url vs client_id params (result_url seems to be tied to classic login experience, confused about why this works since we use the new login experience)

--


For the email verification in brand portal, here is what I am envisioning. Nothing too crazy, but this will require an extra screen for all existing os-frontend users with unverified emails.

New self-signup users:
- Enter user details in self-signup page
- On success, we show a custom "please confirm your email" screen. 
- They click on the email magic link, email is verified, and they are redirected to the brand portal login screen

Existing sandbox users with unverified emails:
- Upon logging in to brand portal, they will see a full screen landing page prompting them to verify their email in order to proceed into the app. This screen should include a button to re-send the verification email.

No changes to new / existing users in bond os.

Changes required:
- add confirm email prompt screen to self-signup login
- add screen to enforce confirmed emails in brand portal with option to resend email verification
- backend / frontend invite flow updates to ensure users are redirected correctly to bond-os or brand portal depending on where they were invited from.