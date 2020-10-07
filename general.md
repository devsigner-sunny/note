## Wordpress - Grabaloan go live
1. Backup live database
2. Enable maintenance mode
3. Git merge stage to live
4. Copy database over to live
```
ssh grabaloan-master@lagoon 
```

- To do that, you may need to change config file first
```
Host lagoon
  Hostname ssh.lagoon.amazeeio.cloud
  Port 32222
  StrictHostKeyChecking no
  UserKnownHostsFile /dev/null
```

5. Copy files over
6. Search and replace url
7. Make site live