# Error solution
---

## General way to replicate issue
- check the cli.dockerfile, install.sh, compile.sh
- try to follow steps excatly same in my local (outside / inside of container both)
- eg) remove all node modules, and follw the steps (install.sh -> compile.sh)
- to make same env, remove the line "  - .:/app:delegated" in docker-compose.yml file. otherwise, it will be delegated
- if both way does works, you have to make sure your cli.dockerfile step is right.

## when lagoon doesn't have or lost compiled css file, what you have to check
1. linting
2. flush cache
3. aggregation
4. Lagoon -> setup.sh


# Launching process
## Email setup
- get the sengrid acceess key
- need to add lagoon variable into lagoon (only avaiable who has access)
- change the value (eg. $USERNAME, value / $PASSWORD, value) 
- above steps, normally gareth will do. so we just need to preapre sengrid access key and send it to him 