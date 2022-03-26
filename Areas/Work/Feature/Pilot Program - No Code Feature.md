https://bond-financial.atlassian.net/wiki/spaces/PRM2020/pages/1672249637/Pilot+Program

account updates (add to tickets):
- make button full width
- make text coopy 14px for reset
- update secondary button to use grey hover state
- set a max width on the whole account page. should apply to the white card and the header content.


pilot program test checklist:
- from scratch
	- signup new user
	- login and get bearer token
	- update all gql calls with the token
	- update all gql urls to correct branch
	- look up my existing customer
	- delete the customer address
	- delete the customer
	- process
- reset pilot for existing customer
	- look up pilot user state
	- delete customer + customer address
	- delete customer_id, card_id, card_account_id from pilot user state

todo:
- do BE kyc sad path ticket
- merge account edit bug
- double check if it is a problem that the customer email does not exist on the customer object created in the pilot program. where does kyc approval email get sent?
- add jira tickets for tasks from dave review
- focus on FE integration
- confirm- when a person has their documents approved through persona (or approved manually) ensure that they will receive an email. is there anything else we need to do here?

card accounts api:
- figure out how to test the full flow in deployed env (as a fallback, will need to hardcode my apikey and make calls locally to real services)
- make sure all acount,card,kyc uuid are stringified


dev up to date pp user:
- timothy+pilot6@bond.tech
- Bond362036!

staging live user details:
- timothy+pilot@bond.tech
- Bond36203620!

--


dev QOL improvements:
- how to format docker python terminal messages?

sandbox dev api key:
- auth: i6v52m13pqVwonyqj1MuekdRgM3YJtv2+abW3T7RUz9zonSzs4pJTbZFcGoBj7x0
- identity: e0f74ecd-0845-40dd-ac33-849643a992d5

bond_test debit program sandbox:
- b3b1fcb0-1ea8-445d-8863-af0ebb527011



pr feedback:
- do we need middle name? we don't have this field anywhere in the current forms. should this be reflected in the ui?
- ssn is part of customer creation?
- does kyc update the customer object and intialize kyc seperately
- reverse date format


----

Notes from Yang 1:1 on Feb 9,

resources to learn about programs:
- figure out which program to use when we issue virtual cards for pilot program users (we potentially share with bond reserve)
- https://bond-financial.atlassian.net/wiki/spaces/PRM2020/pages/1335328770/PRD+Program+Management
- https://bond-financial.atlassian.net/wiki/spaces/PROD/pages/831356929/Brand+Config+and+Display+Interface+within+Bond+OS
- https://docs.bond.tech/docs/bond-studio-resources
- https://bond-financial.atlassian.net/wiki/spaces/BAAS/pages/1434877959/Configuring+Brands+and+Programs


in regards to sharing a progam sharing bond reserve:
- virtual card vs physical wth reserve is difference in program reqs if we share the same existing program. reserve obviiously uses  physcial and virtual. could this be shared?
- how do we allow multiple pilot users to still view their card / transaction info under another "clientID" if the card is issued to the bond reserve program?

other notes:
- talk to andrew to get up to speed on program constraints, how it works
- the ideal path would be for us to share the program (i need to understand why not spin up new programs for each user, or one broad new program for all pilto users)
- if sharing program, how does it show up for all brands if its under bond?
- do we create a new bond rserve codebase, or do we share code with exsting? concern is that bond reserve is used for dogfood and might be more stable. yang open to us just copying code over and creating a new service. 
- yang can help on the backend here
- she wants me to take a lead on this, estimate resources on FE and backend days once we have scope locked in an answer the main questions above (program approach, shared or new reserve codebase). wants me to take lead. she wants my help to understand the effort involved.
- if bond reserve is a different program, how does deployment work?

action steps:
- learn about programs and approaches
- learn about bond reserve codebase, approaches to sharing or spinning off
- set up a time with andrew chen to walk yang and me through the reserve codebase
- circle back to discuss with yang early next week. ask her qs in meantime if needed.


--


Discuss with Yang next:
- can we lower the threshold for tests, mocking out every single api with real payloads. this is way more thorough and time consuming than our past approaches. i estimate this adds 30 percent more dev time in based on brand portal. is it worth lowering that to get this out sooner? I'm not sure that the level of effort with unit testing is going to pay dividends at this stage (i think e2e would be higher value for instance)




self notes from group chat:
- we need to add KYC in the QS flow
- how do we store state regarding how far a user is into the quick start flow? do we store this in the db?
- is quick start user level vs brand level? should every user see it? 
- how do we migrate existing self signup sandbox users?

program, bond studio program, i2c
- how do we preload debit card
- how do we make sure all the steps in qs can be done programatically? check with kirby.
	- is there any issue pulling the card info, transaction info for a user's card that is from a bond reserve program if the user themselves is under a different (their own) brand?
- how could we freeze program if we need to shut it down due to fraud

--

Actions for Friday Feb 11:
- look at bond dollars fe/be code to get up to speed on card creation / program aspects
- trace the relationship between fetching card, transactions for a client. whether the bond reserve program would be scoped to just bond users
	- look at endpoints to fetch cards
	- look at endpoints to fetch transactions

questions for kirby:
- approaches for programs? can we create programmatically? should they be bundled together?
	- crux of the question right now: 
		- we need to be able to programmatically issue prepaid debit cards
		- card info, transaction info needs to be accesible from within their brand. ideally analytics as well.
		- we need to be able to set some global thresholds /monitoring on the programs. need to be able to pull the plug on the program if abuse gets out of hand, with ideally minimal impact to those that already have their cards.
	- help us understand how bond programs link to i2c programs? does i2c config live on their side? how do we differentiate at the program level? (just see i2c listed as option)
	- any issues with having multiple brands share a program for this single step?
	- how could we flip a switch to shut down cards for specific users? how could we shut down access for all users?
- how do we preload the card with 20 bucks?
- talk through fraud issues, what measures they took to get a handle? what steps can we reasonably take?


questions for yang:
- why not just add this code to os-backend? rather than creating a new service.



notes from chat with kirby:
- crux lies around all pilot users under same brand, or new brand pointing to same i2c
	- if they are same brand, we just restrict them to functions that are for one card, none fro brand level.
		- we would not have to worry about programatic creation of program, we just hardcode this part for the pilot prgram
	- if we put these randos on bond reserve program, that is going to mess up all those dashboards that pull from that wholistic program. we also can't put threshold limits without affecting all bond reserve users.
	- by cerating a new program for pilto program we can impose restrictions at program level without affecting others.
- can link external accont to specific card, no concerns here
- if we migrate from cbw to evolve, we create a new program and on card create, just point them to new program. we need to store the user -> program mapping in pilot program user state.
	- the old cbw users could keep operating as expected without users.
- preloading debit card:
	- create card
	- and then fun the card in second call
- limits on spending is exclusively at i2c program level. current work done on credit card limits from our end is in progress but might not work for debit cards.
- we need to consider ach funding limits, its a 20k a day or something liek this.
- card expiration is set at a program level. if card expires with money still on it- money gets stuck there for the user. 
	- there is a way for us to reclaim the money.
- need to clarify how we transition to v2, linking users own account


--

questions for helcio:
- free money will absolutely mean there is sophisticated fraud done at large scale. we will put a target on our back, and there are large communities that will revel in making out with as much free money as possible. that is just inevitable. what happens when that money runs out quickly. 
	- what is our budget for this? what happens if the budget is drained quickly? what is the user experience? what happens to folks that followed our marketting about the pilot program but we have been forced to shut down quickly?
	- if the goal is to build confidence through making real transactions, what happens when word spreads that large scale fraud happened on our platform?
	- is see the value if we can issue free cards to genuine prospects, absolutely. but i think the path to success is very narrow and fragile- the premise here is to generate confidence with real money. it is going to be predicated on us keeping this open for long enough to reach the users we care about, while eating the cost of massive waves of fraudsters. i think we need to be realistic that there is a very good chance we will get swarmed and wil need to shut this down quickly. we could try to pivot, we could try to experiment with additional measure, but i do have serious concerns about our abilityu to pull this off with longevity. i understand the value prop here, i want to be clear. but i am very worried about our abiliyt to accompolish this and to deliver the value.
		- i also see the risk of reputational risk or negative press being quite high. so in some ways this feels like a very risky play.
		- are we thinking ahead to what we would do if we determine issuing real cards is not tennable? can we pivot this to veritual cards? what happens to the user experience (and the marketting) if we quickly get swamped and have to put the program on pause?
		- what gives us confidence that this is the most important thing right now? why not start tackling the no-code / low code solutions that woud also serve the purpose of making it easier for a user to create customers, issue cards, quickly? why is this more important?



- I see the value we here. If we are able to get these cards into the hands of real prospects, it is a great showcase about what is possible with our platform.
	- My concern is regarding our path to success. I think it is quite high risk, and I'm very concerned about our ability to hit our success criteria with the current plan. 
		- We can our do our best to apply common sense mitigation efforts when it comes to email verification, requiring KYC. But in my opinion, it is still highly likely we will get swarmed by sophisticated users who can spam our system at a really high scale. So far, I'm not convinced that any of the measures we have at our dispoal we can say will confidently prevent whatever money we have here getting very quickly drained.
		- I want to talk through what we will do, and how we are thinking about our response if this gets swarmed and drained quickly.
			- From a product standpoint, we don't really have a plan B here regarding a version of the pilot program that is not based in issuing real cards. so if we have to shut it down or suspend it, do we end up wasting the engineering efforts here.
			- I think there is also this question of marketting efforts, what we would do if we market and then have to shut it down
			- how do we think about repulational risk: if word spread, if we become known as a company that got exploited. it seems like there is a risk that
	- why not move directly to working on nocode / low code? why risk the pilot program?



--

