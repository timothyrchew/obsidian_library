Findings:
- dev / test branches fail with the same baas_config error on signup. issue was introduced after the move off dev3.
- os-fe connected to dev works fine. so the issue seems to be limited to baas_config on signup.
- I can make requests to services via curl/postman with kubefwd without issues
- os-backend fails when trying to use any kubefwd urls (with the same error that I see in test branches, although could be totally different reasons)

Need to figure out:
- do test branches fail to connect to ANY services, or just baas_config. (just point local os-fe to deployed test branch and see if stuff works or if everything breaks)

Next things to do:
- try out deployed test branch with local os-fe to see if other services are breaking as well
- confirm with yang if she has got os-be working with kubefwd, if she uses containerized or via local python env.
- try to figure out if local os-be fails for the same reason as test branch, or if it could be unrelated. talk to nick about why using kubefwd might fail with containerized os-be.
- questions for nick- if its the correct url passed into `requsts` module, what else can i look out? where else should i look?