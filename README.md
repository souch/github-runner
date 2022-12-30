## github-runner


### runner-auto-update
inspired from https://github.com/tcardonne/docker-github-runner
```
cd runner-auto-update
make help
make build
make start ACCESS_TOKEN=mygithubtoken REPOSITORY_URL=https://github.com/mygithubaccount/myrepo
```

### simple runner
inspired from https://github.blog/2020-08-04-github-actions-self-hosted-runners-on-google-cloud/
```
cd runner
make help
make build
make start TOKEN=mygithubtoken OWNER=mygithubaccount REPO=myrepo
```
