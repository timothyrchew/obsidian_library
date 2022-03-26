# Next



 example of what i need to do with the nested fields: https://www.django-rest-framework.org/api-guide/serializers/#writable-nested-representations


Chat with Dennis
- double check on timeline for this. definitely taking a lot longer than i expected with a couple roadblocks and new stuff to dig into.
- how to handle the remote_id issue
	- first can't understand why there is a class of a unique key
	- second, confused about how the serializer handles this case
		- we don't want to expose this key, so it wasn't included in serializer fields
		- but then this means that when you pass in fields to save
- confused about the default serialization process from "through" relationships. field names are changed, and it is just returning the unique field from the intermediate table, not the faction info.
	- what is the test strategy for this when i need to build the payload to test the create action (recognizing this isn't the issue with real use but more of a testing issue, following other examples of how this is done)
	- do we need to change the default serialization process here?
- strategy for handling images in the factory- tempfile or SimpeFileUpload? do we need additional validation beyond what the model provides (it seems to be fairly robust as is)?
- should validation handle the nested models? if so, what would we want to test there? ive added all the hard contrainst that wouldnt be caught by the default validation based off the model.



- debug hdmi issue with my work laptop
- learn how to use more features from vscode, how to run debugger in the IDE. how to run single funtion, setting breakpoints, etc. circle back with dennis if i need more help here.
- do some performance debugging, figure out where our bottlenecks are in the react app


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
