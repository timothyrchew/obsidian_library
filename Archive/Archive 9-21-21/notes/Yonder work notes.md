





# Next




e2e in docker painpoints:
- we can't use the playwright inspector tool or run the tests in anything but a headless mode
	- no setting breakpoints
	- no watching the tests play out to see where it gets stuck
	- no utilizing playwrights ability to record scripts
- we can use the trace viewers, which records the actions, saves them to a zip files, and lets you play them back. but you its entirely retrospective, so you can't utilize breakpoints. and you have to manually call a stop record point, so when it comes to debugging async issues (which is a huge part of of e2e debugging), its not terribly useful for this.
- with the current approach, we more of less have to write and debug these via trial and error. im not sure of the ROI looking a us getting good enough test coverage this way. i can say just from working figure out why the auth0 loging / redirect is failing- can be really time consuming / inefficient.
- once we have our e2e tests written, we can run them in CI fine. but the challenge is getting them written without having access to all the tooling if we are bound to actually developing them within docker.


how ive seen other people tackle this. its fair to say that cypress has the most traction right now.
- cypress has a pretty detailed section on seed data strategies. cypress "real world app" example also has a full example of an implementation:
	- a couple approaches:
		- (the approach we are trying to take- manage seed data from wihint the tests) there are a couple cypress commands to run 'plugins', execute system commands, or make http requests in order to manage seed data
			- they use npm commands to do basic seed data setup
			- store seed data as json on the frontend
			- and then they define a cypress plugin to query and modify seed data as they go- all via a dedicted /test api endpoint. this keeps the e2e suite and test runner completely decoupled from the backend.
		- they also talk about stubbing the backend entirely.
		- "true e2e": where you doing don't stub anything- although they admit if you do this its probably just for a couple tests and you will want to stub the rest.


e2e test options:
- stick with the current strategy:
	- continue down the same path, brute force approach to writing e2e tests without the extra tooling. there is a path forward, it will just a be a painful one that frankly im not to excited about.
	- we find someway to setup a proxy so that playwright itself can run outside of docker, while being controlled by pytest inside of docker.
- we narrow the scope of e2e tests- focus on auth and permissions, which are arguably the most important. we decouple e2e from running within docker, and just do an initial seed with a couple different reports and accounts. then test the different paths without having to modify the seed data on the fly
- we take the cypress approach. have our e2e tests decoupled from pluto. our e2e tests handle seed data setup / teardown via system commands and via dedicated test api endpoints.







- debug hdmi issue with my work laptop
- figure out how auth0 works, what tennants are, how it interfaces with our django auth. why i can't flip db and connect from local
- come up with some better approaches to getting real data into local db. how to connec to pods. how to do a smaller scale datadump.
- learn how to use more features from vscode, how to run debugger in the IDE. how to run single funtion, setting breakpoints, etc. circle back with dennis if i need more help here.


# Notes



http://localhost:8000/reports/daily-reports/alex-test-0/2021-06-20/edit
http://localhost:8000/reports/api/daily-report-content/109f7b8c-22f7-4abb-87fc-a5092b2d0a65/narrative/narrative-detail




```
dev.config.env


DEFAULT_URL=http://localhost:8000
DJANGO_SETTINGS_MODULE=reporting.config.dev
DJANGO_ALLOWED_HOSTS=localhost,reporting.local
VIRTUAL_HOST=reporting.local
VIRTUAL_PORT=8000
CELERY_BROKER=redis://redis:6379/0
CELERY_BACKEND=redis://redis:6379/0
PG_HOST=db
PG_PORT=5432
PG_DBNAME=postgres
SF_ACCOUNT=bh44633.us-east-1
AWS_REGION_NAME=us-east-1
KUBECONFIG=/opt/dev.kubeconfig.yaml
REPORT_JOB_MEMORY=4Gi
REPORT_JOB_CPU=1
APP_HOST=reporting.local
AUTH0_TENANT_DOMAIN=newknowledge-sandbox.auth0.com
AUTH0_AUDIENCE=urn:auth0-authz-api



```


```
dev.secrets.env

SF_USER=summary_report_dev
SF_PASSWORD=ZAAzsV4yZh2xXsC8mxCo6fBN3

PG_USER=postgres
PG_PASSWORD=admin

CONSUMER_KEY=OT8yFfP3PGQRV9TDkL4zJO4RJ
CONSUMER_SECRET=5jTzBdQvE7ECAE2sU0fuUGz9ElWSIq70QGyq1cInb4GYFwvnXt
ACCESS_TOKEN=8392632-6BR1CHGoD82w89SfbVbUyFXEO2iClk5KTvPKyUBgIg
ACCESS_SECRET=W5pklr4cnYnYGvHk3kdvVS9mVQSejnLDckovVUegT5Qah

URL_PING_API_KEY=P78CBF811D34B63
URL_PING_SECRET=S_C81C30B99ACE9

DEBUG=on

SECRET_KEY=mysecret

AWS_ACCESS_KEY_ID=AKIAVJEST7S4BSU4U2EN
AWS_SECRET_ACCESS_KEY=zdSPJU26T3HqAj7Z9oGLsR7s5kCX4WJxT54aifyt

EMAIL_HOST_USER=unused@example.com
EMAIL_HOST_PASSWORD=unused-in-dev-but-must-be-non-null-for-minikube

AUTH0_SECRET=RJqmJPxMQxYxTkux_WGPsr4LSLJB0JPqF4h-Z3EmRCR6_UbINcjAbHjFaKqlNukn
AUTH0_M2M_CLIENT_SECRET=ZpQdcgc50RwE3QVVtAxlxpbzMgjf25QD1SwiFL8fml_yb-wcnkCly-2Y12FApWa8

AUTH0_CLIENT_ID=x67bvhhJwfnJl5NME9mZjwVil76okpkI
AUTH0_CLIENT_SECRET=ZpQdcgc50RwE3QVVtAxlxpbzMgjf25QD1SwiFL8fml_yb-wcnkCly-2Y12FApWa8
AUTH0_EXT_URL=https://newknowledge-sandbox.us.webtask.io/adf6e2f2b84784b57522e3b19dfc9201/api

```



# Charts


# stuff to learn / better understand
- understand brew, dig in and resolve errors generated from `brew doctor`
- resolve the seeminly multiple version of pyhon, as well as conflicts between brew python / pip and native version, as well as bad python alias. also solve where i install django when following default instructions from django tutorial.
- figure out why pip install is not adding packages to venv
- understand OAuth, Auth0 and how these techniques work behind the scene
- understand how to read datadog logs
- understand docker and kubernetes
- learn better sql
- learn bash scripting
- learn regex
- learn how ssh works and mechanics behind `ssh-add` and your agent pid
- understand pipenv and configuration
