
master password:
Bond36203620

dev api:
`https://os-api.dev.bond.tech/api/auth/user/sign-up/`

new dev api cluster: (use this for os backend test branches too):
`dev-use2-1`
ie: `https://os-api-test-pilot-card.dev-use2-1.dev.bond.tech/api/auth/user/sign-up/`

kubefwd:
`sudo kubefwd svc -n studio-sandbox -n studio-live`



os-backend clean install:
- ensure clean branch
- restart terminal, wipe out global artifactory vars from brand portal that might interfere.
- git pull baas_api, make docker, make generate-all
- `docker-compose --file docker-compose.arm64.yaml build` && up

```
PGPASSWORD=postgres psql -U postgres -p 5435 -h localhost -f test_db/local.sql
```

seed user data from interactive session:

PGPASSWORD=postgres psql -U postgres -p 5435 -h localhost

```
insert into auth0_user (sub, email, given_name, client_id) values ('auth0|61522f00fe39bb00691d7655', 'timothy@bond.tech', 'Timothy', 'abaa2147-4533-4ec7-a9f6-bd88a988ecd1');

insert into user_info (sub, last_used_env, status_id) values ('auth0|61522f00fe39bb00691d7655', 'live', (SELECT status_id FROM prm_user_status WHERE status='Active'));
```


OS-backend m1 kafka issue:
- `librdkafka-dev make` add to dockerfile `apt-get`
- https://github.com/bond-tech/baas_webhooks/pull/165/files