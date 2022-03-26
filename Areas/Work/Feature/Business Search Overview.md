Body of work:


FE:
- nail down David's designs
- create feature flag for this work
- add new search business search params to dropdown
- update results modal to have unified fields for both customers + businesses
- handle conditional rendering of customer vs business details page
	- conditionally resolve business vs customer details query based on return entity type
	- different top level business data, different card tabs, different account data fields, new table col if parent card
- add new `get_csr_business_details` query
	- include utility to assign parent/child card property to all returned cards
- create new dropdown card select element
- create new "view more" search element modal to handle "view more" cards if higher count
- update transactions table to query by business id if fetching parent card for business
	- add new last 4 col if showing parent card


BE:
- search
	- (os-backend) expand `get_csr_customer_results` to include business params
		- (os-backend) extend baas_api `get_customers` method to include businesses from internal api (if having business_id) or querying `os-search` if by business fields
		- (baas_person) extend `/internal/os-search` to include business entities if business fields passed in
- fetch business details
	- (os-backend) add new `get_csr_business_details` query resolver
		- fetch business info from baas_person `/businesses` by business id
		- fetch cards from fincore by `/cards` with business id
- fetch transactions
	- (tqs) enable fetching all transactions for a business_id

--


Takeaways from Yang:
- she wants to be in meetings that have to do with scope
- we won't touch fincore
	- its on tory- if she wants to go with the shortcut now, its on her to determine ownership for the future
		- if she wants to do it now, then she needs to get another team to tackle the pieces


Tory / Yang:

Summarizing where we currently stand regarding options for fincore backend work. All this work here will need to by the treasury team:

Option A:

Scope:
- only 1 parent card per business
- business will not exceed ~100 child cards
- transactions for de-activated cards will show up under parent card transactions

Backend work:
- Update TQS to support fetching all transactions by business ID

--

Option B:

Scope:
- support multiple parent cards per business
- support high child count

Backend work:
- Create internal `/cards` endpoint
- Update TQS to support fetching child transactions with a parent card's transactions (5 - 8 days)



