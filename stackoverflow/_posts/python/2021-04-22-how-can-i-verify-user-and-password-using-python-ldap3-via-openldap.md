---
layout: post
title: How can I verify user and password using Python ldap3 via OpenLdap?
image: https://i.ytimg.com/vi/QyhNaY5O468/maxresdefault.jpg
description: >
  For a django project, I designed a different login page. The users here will log in via openldap. I can access users' full information with their uid id, but I could not find how to verify the password.
hide_description: true
hide_image: true
comments: true
last_modified_at: 2021-04-22
categories: [stackoverflow]
tags: [python, django, openldap]
---

How can I verify user and password using Python ldap3 via OpenLdap?

{:.no_toc}
1. this unordered seed list will be replaced by toc as unordered list
{:toc}

## [Question](https://stackoverflow.com/questions/63808692/how-can-i-verify-user-and-password-using-python-ldap3-via-openldap) by [Birol Emekli](https://stackoverflow.com/users/10721248/birol-emekli)

For a django project, I designed a different login page. The users here will log in via openldap. 

I can access users' full information with their uid id, but I could not find how to verify the password.

Do I need to hash the user's password and compare it with the password on ldap? Isn't there any other method? Thank you

  
```py
from ldap3 import Server, Connection, ALL, SUBTREE
from ldap3.core.exceptions import LDAPException, LDAPBindError, LDAPSocketOpenError
from ldap3.utils.conv import escape_filter_chars

ldap_server_uri=f"ldap://xxx:389"
ldap_base = 'dc=xx,dc=xx,dc=xx'

def ldap(uid,password):
    try:     
        ldap_server = Server(ldap_server_uri, get_info=ALL)
        ldap_connection = Connection(ldap_server, user = 'uid=admin,ou=xx,dc=xx,dc=xx',password='adminpassword')
        if ldap_connection.bind() == True:
            if ldap_connection.search(search_base=ldap_base, search_filter=f'(uid={uid})',search_scope = SUBTREE, attributes=['uid']) == True:
                ent = ldap_connection.entries[0]
                entry = {'uid': ent['uid']}
                ldap_connection.unbind()
                return entry
            else:
                return None
    except LDAPSocketOpenError:
        print('Unabled to connect to the LDAP server!')
        return None
```

## [Answer](https://stackoverflow.com/a/65907735/13155046) by [Milovan Tomašević](https://stackoverflow.com/users/13155046/milovan-tomašević)

Just to check the username and password I use:

```py
import ldap3
from ldap3.core.exceptions import LDAPException


def _ldap_login(username, password):
    try:
        with ldap3.Connection('enter_server', user=username, password=password) as conn:
            print(conn.result["description"]) # "success" if bind is ok
            return True
    except LDAPException:
        print('Unable to connect to LDAP server')
        return False

_ldap_login("enter_username", "enter_password")
```



The following are [30 code examples][1] for showing how to use `ldap3` and [Tutorial: Introduction to ldap3][2].


  [1]: https://www.programcreek.com/python/example/107944/ldap3
  [2]: https://ldap3.readthedocs.io/en/latest/tutorial_intro.html#tutorial-introduction-to-ldap3