# create user named kuma

give kuma complete access to sudo, nopasswd

show that kuma user has sudo access

change kuma user (and group) names to something else

create new kuma user

show that it now has sudo, even though the user IDs don't match, the sudoers rule was for user names and not user IDs

(the new kuma user doesn't own it's home dir and stuff, but it has sudo so it can `chown` them over to itself)
