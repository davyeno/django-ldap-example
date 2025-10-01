# django-ldap-example

# Add test users
docker exec -it smblds sh

# create an OU to keep dev users tidy
samba-tool ou create "OU=DevUsers,DC=example,DC=local"

# create users
samba-tool user create alice 'Passw0rd!' --given-name=Alice --surname=Tester
samba-tool user create bob   'Passw0rd!' --given-name=Bob   --surname=Tester

# move them into the OU (optional)
samba-tool user move alice "OU=DevUsers,DC=example,DC=local"
samba-tool user move bob   "OU=DevUsers,DC=example,DC=local"

# (optional) create a group and add members
samba-tool group add Devs
samba-tool group addmembers Devs alice,bob


ldapsearch -x -H ldap://localhost:389 \
  -D "administrator@EXAMPLE.LOCAL" -w Passw0rd! \
  -b "DC=example,DC=local" "(sAMAccountName=administrator)" dn sAMAccountName

ldapsearch -LLL -x \
  -H ldaps://dc1.example.local:389 \
  -D "administrator@EXAMPLE.LOCAL" -w 'Passw0rd!' \
  -b "DC=example,DC=local" '(sAMAccountName=administrator)' dn sAMAccountName

