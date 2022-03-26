Check:


--
SANDBOX DEV
api identity:
70a133dd-3bd0-466b-80ff-b1e0de6acf96
auth:
yWR5xZxgSiMRDZk+yGjYbOfBCbEUwkcBtP7jaip2jlnAb96Y/hVHo7FSAlCKOcZz


LIVE DEV
identify:
1ef9c21c-55b0-4d7e-bdd2-c0aa0a555dd9
auth:
OzRO1lsxBwnQzGHRBaiizRxToAmTYBnnITuQrqeG7stECAtZ+JbaKJEXNJ41pHze


--

PROD SANDBOX
identity:
86856c61-d5ff-463b-8fa2-9a13da80d6a0
auth:
ENBjwAVQiDNcd57xDdRoOWNY8YRvAr05SM6h6FdFuZhn78VYiwJp6h+ISawaCWst


Steps:
- figure out if we have CSR data in dev sandbox so we can test this out locally.
	- install bond-connect, check dev sandbox data
	- if no data, i can look at seeding via the api key 
- build out tickets for this work


test approaches:
- local
	- point to dev dbs
	- try out yangs local setup
- in prod
	- hide CSR behind feature flag
	- create a sandbox self signup user and test with featrue flag enabled

# Current support

os-backend:
- resolve_get_csr_customer_results & resolve_get_csr_customer_details
	- gql queries resolve to sandbox by explicitly setting `isLive` boolean
- tqs resolves via rest call w/ header
	- appears to route to sandbox version based on `X-Selected-Env` header
