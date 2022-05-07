# go-xmpp example

Working example with STARTTLS enabled.

## Ejabberd setup
```
podman run --name ejabberd -d -p 5280:5280 -p 5222:5222 ejabberd/ecs
podman exec -it ejabberd bin/ejabberdctl register admin localhost admin
podman exec -it ejabberd bin/ejabberdctl register james localhost james
```

### See everyone online
1. Login at http://localhost:5280/admin and go to 'Virtual Hosts' -> 'server' -> 'Shared Roster Groups'
2. Create a new Shared Roster, with identifier 'EveryBody'. Click on 'Add new'.
3. Click on 'EveryBody' and fill in the following ('Name' and 'Displayed Groups' are case-sensitive):
   - Name: EveryBody
   - Description: This group contains everybody
   - Members: @all@
   - Displayed Groups: EveryBody
4. Click on Submit.

### Login as user
go run example.go -server=localhost -username=james@localhost -password=james -notls=true -debug=true

### Anonymous login (optional)
`podman exec -it ejabberd vi conf/ejabberd.yml`
```yml
hosts:
  - localhost

auth_method:
  - internal
  - anonymous
```
Reload config:
`podman exec ejabberd bin/ejabberdctl reload_config`
Login:
`go run example.go -server=localhost -notls=true -debug=true`
