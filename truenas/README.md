# TrueNAS

## App users

| UID  | NAME      |
| ---- | --------- |
| 3000 | lidarr    |
| 3001 | prowlarr  |
| 3002 | sonarr    |
| 3003 | tdarr     |
| 3004 | radarr    |
| 3005 | overseerr |
| 3006 | sabnzbd   |
| 3007 | plex      |
| 3008 | anytype   |
| 3014 | immich    |
| 3015 | anytype   |

## Scripts

### Query user

`midclt call user.query '[["username", "=", "[username]"]]' | jq`

### Create app user through midclt

`midclt call user.create '{"username": "[appname]","full_name": "[appname]","uid": [appuid],"group_create": false,"group":[app group id, e.g. 93],"home": "/var/empty","shell": "/usr/sbin/nologin","password_disabled": true,"smb":false}'`