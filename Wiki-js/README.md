# This compose file is made assuming you have an existing postgres insstance on your server. 

If you don't, run:

```yaml
sb install postgres
```
The default user i believe to be saltbox, it could be your server user from `accounts.yml`

# To administrate your PG instance, install PGAdmin. 

Run:

```yaml
sb install sandbox-pgadmin
```

Pgadmin is listed in the sb docs. You can create a db user, db pass, and a db from there. 
