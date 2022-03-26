notes from yang about roles work:
- wants to be able to dogfood brand portal
- all bond os users should be able to log into brand portal as brand users under a 'bond' brand. we need to make sure they are treated the same.
- yang says this will likely involve some code changes, because we have api calls that are hardcoded to treat bond users differently than other brands. we need to make sure that in brand portal there is parity and bond users have same experience as brand users
- if it willl be too much work to support this, yang is open to hardcoding something in brand portal to ensure bond users have the same experience as brand.
- this should be part of the requirement for user login story

Brand Portal Bond Roles
- bond brand differences
	- client/queries: fetching clients for the dropdown. if bond, it will return all clients. (do we even need this in brand portal if there is no switcher?)
		- get clients, get programs
	- we have `client_type` field on client table- brand, band, or bond.
	- graphene middlewhere does client_id check

two different problems to solve here:
- standardize the way that we ensure a request is scoped to the right brand
- update the way that some endpoints implicitly have different behavior for bond client vs other brands. 

we are doing a couple things:
- one is implicitly treating bond client differently and returning data for all brands
- allowing a bond admin to masquerade as a brand user (change_brand_id)
- permissions checker

graphene middleware ensures that you can only modify your brand id- obvi just for graphql
isBrandIdAuthorized: for restful routes.
change_brand_id: if passing in client_id, it will overwrite the user context. just used for restful routes right now.

improvement:
- we could consolide the code inside the middleware and decorator to share the same logic. from what i see they are doing the same thing with slightly different params. i think we could remove the graphql middleware and use the same permissions class.
- can we unify the way that we check for bond brand? i think maybe we remove the implicit bond means fetch everything. endpoints accept an `fetch_all` paramter, we could extend the brandId checker to ensure that user has permissions.
- what can we pull from the auth0 token


two different problems to solve here:
- standardize the way that we ensure a request is scoped to the right brand
	- this is being done in 3 different ways that ive seen. this is partly tied to different implementations with graphql and restful routes. but this could be consolidated further to share the same logic.
- update the way that some endpoints implicitly have different behavior for bond client vs other brands. 
	- we could standardize this to just return values scoped to the client as default. and then accept a standard parameter that would return all the brand info with this payload. could check that user is authorized via a decorator to ensure that they have permission to requeste all.





notes from follow up chat with yang:
- she would like us to consolidate the logic that manages brand permissions for both graphql and the rest. should be a somewhat minor change to reduce the duplicated logic here and to share the core logic between restful permissions and the graphql middleware to ensure user has access to the brand.
- she also said that the change_brand_id is not correct in that it will set a client_id that doesn't exist on the user (although im not sure i agree that it is not what was intended)
- she things about access control in three levels: the highest level is feature flags (global). next is client acess- having to do with the data itself. and then the lowest level is role based- what a user can do with that data. we should be focused on the second two and we should ensure.
	- part of this work should also be to think about how make extensible roles that can be applied to all our routes. a bigger part of this has to do with the RFC koty is working on. but the same approach should be ironed out for backend, either role based but also potentially more granular permissions based. she envisions that we will need scopes beyond auth0- thinking about permission based on whether you opted into TOS or opted into the beta for something. so we would need to store our own table with users state. i should be thinking about this and how we would do on the backend (although i think we are actually in a decent place on this now).
- she is mostly concerned about access control here and roles for this work. specifically, we shouldn't rely on the clientId passed in as an argument, but she wants to pull this from the JWT token. in the domo example, it is pulling the clientid from the argument for bond users, but for brand users the clientId is implicit based on the jwt (you can see that the clientID param is empty). this should be standardized- functionally this means that routes for the brand portal should not pass in a client id, but rather it should be pulled from the jwt. however bondos should continue to behave the same.
	- I think the main reason for this, is so that we can standardize our access controls via diecorators. because we do a mix of clientid via diferent named arguemtns and sometimes do it just by token, there is not consistency. we want to make it standard to check it off of the token.
	- specifically for the routes that serve the userprofile page, or the brand portal currently, she is good with us making the updates incrementally. routes should pull the clientId from the token, not from the required argument
- actions
	- clean up the brand id permissions checker: share the logic between gqp and rest.
	- backwards compatibility: we need to still support client argument if it exists. fallback to the jwt clientid.
	- masquerading as a different user should be checked via a gloabl decorator

--

summary of brand access approach:
- endpoint implementation
	- for all brand users, the client_id should be implicit and pulled from the token.
	- bond users should be allowed special access (needs custom handling at each route level)
		- in some cases they need to fetch all resources (ie for the client switcher)
		- should be able to masquerade by passing in a client_id argument different from implicit token client id
- access control enforcement
	- apiview permission_classes 
	- gql middleware
- roles enforced
	- decorators exist for both rest & gql

current problems / gaps that should be ideally addressed in the future:
- standardize explicit client_id argument. this is super inconsistent across different parts of the app (ranges from client_id, brand_id, selected_client_id, bond_brand_id)
- brand portal should not pass in explicit client_id where not needed (might require updates to corresponding backend routes). due to the fact that we need backward compatiblitiy with os-frontend, routes should allow client_id to be implicit, but will probs still need to support explicit client_id. id = token.id || argu.id.
- for individual route implementations, could look creating utilities to standardize the "if bond user do this flow, if not do this flow" checks.