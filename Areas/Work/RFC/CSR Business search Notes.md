

PRD link: https://bond-financial.atlassian.net/wiki/spaces/LED/pages/1489109108/PRD+CSR+Phase+2

Purpose & Motivation:
- The CSR portal currently only supports looking up individual customer accounts.
- For brands servicing business accounts, there is not the ability to search or view account information within the CSR portal.
- This RFC addresses an MVP for business account search as one part of PRD.

Requirements:
- MVP: a user should be able to look up a business account by the DBA name or last 4 digits of the PAN
- Follow on: should be able to look up business by EIN, legal business name, beneficial owner name, domain name (might not be any harder to just tackle this all right now. doig by DBA and other fields is same approach, minus the domian)

 
API updates:

  
`resolve_get_csr_customer_results` query resolver 

- fincore already returns business cards when queriying by PAN number. we can use the associated business_id for the next step
- we need to make a call to baas_customer `/business` to fetch business data with business_id, DBA, (or other fields) (IMPORTANT: does this functionality exist? not sure it does when querying by anything other than ID.)
- should we keep the customer and business queries using the same endpoint? we are introducing another call for search endpoint to fetch businesses and will see slight performance degredation. (this could probably be alleviated based on what search params where used, since there is no overlap other than PAN, which will we differentiate the results from that query already)
- should we merge the calls into a single baas_customer endpoint that returns both entities from a single call?

`resolve_get_csr_customer_details` query resolver
- can either extend this current to fetch business details if `business_id` is passed in, or can break out different query resolver for businesses (logic for which to call would have to be handled on the frontend)
- i think this should be a seperate query. search results will indicate if customer or business, and the top level display needs to be different so we should probably differentiate.

- currently can only fetch all businsses by brand_id, or can fetch a single business with the business_id. so we need to add a new route to search for businesses by other props. for customers, os-backend has a checker that routes searches by id to the generic rest get endpoint, and then sends searches by other params to the `os-search` endpoint. we need something comparable to businesses. can either expand os-search to include businesses, or spin off a separate endpoint.
	- this should be wrapped under a `get_businesses` helder in os-backend

build_business helper
- solidify which associated fields need to get passed to the frontend

Frontend:
- add the DBA (and other search fields) to the search box
- build a GET_CSR_BUSINESS_DETAILS graphql query
- add component to display the business result


RFC structure:
- read a number of RFC, find a couple good examples and get a feel for different types of approaches. look back at the launch darkly proposal to use as a potential model
- do solidify exactly what the purpose is here. breaking down the work vs proposing a solution and evaluating tradeoffs.
- don't overthink it, don't make it too complex
- do consider tradeoffs
- do consider future state (with the extra fields they want to add later on)
- do ask yang if i have any questions
	- should this have frontend work defined as well? or this concern here primarily raising questions about any approach that might be controvertial or might involve different approaches


my approach:
- go over the existing search functionaltiy. list out the architecture.
- describe the proposed changes


from tory:
- when fetching the parent business card:
	- transactions should display all child children transactions
	- we'd like to have the child cards displayed in the tabbed view (although this UI will need to change because there can be potentially thousands)
- for child card, it behaves the same as customers
- out of scope: showing beneficial owners
- use existing dropdown fields, unified search box between customer / business
- for the modal with multiple search results, unify those fields so that it applies to both businesses and customers. use single "name" fields with customer-id. (this will probably need to change)