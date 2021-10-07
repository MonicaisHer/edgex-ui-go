## Status: 

draft

## Snap test guideline:

#### Reminder: 

- Do not give names in UI with blank space, better with CamelCase. Blank space will have problems when using REST API
- Make sure all buildings as below install within 1 hour, make sure no other snaps or packages are using Redis server, for example, nmap

#### Install edgex-ui snap and edgexfoundry snap:

```
$ snap remove --purge edgexfoundry
$ snap install edgexfoundry --channel=2.0
$ snap install edgex-ui --edge
```

After these steps, edgex-ui and edgexfoundry will automatically running

#### Check:

```
$ snap services
```

we should see:

```
Service                                    Startup   Current   Notes
edgex-ui.edgex-ui                          enabled   active    -
edgexfoundry.app-service-configurable      disabled  inactive  -
edgexfoundry.consul                        enabled   active    -
edgexfoundry.core-command                  enabled   active    -
edgexfoundry.core-data                     enabled   active    -
edgexfoundry.core-metadata                 enabled   active    -
edgexfoundry.device-virtual                disabled  inactive  -
edgexfoundry.kong-daemon                   enabled   active    -
edgexfoundry.kuiper                        disabled  inactive  -
edgexfoundry.postgres                      enabled   active    -
edgexfoundry.redis                         enabled   active    -
edgexfoundry.security-bootstrapper-redis   enabled   inactive  -
edgexfoundry.security-consul-bootstrapper  enabled   inactive  -
edgexfoundry.security-proxy-setup          enabled   inactive  -
edgexfoundry.security-secretstore-setup    enabled   inactive  -
edgexfoundry.support-notifications         disabled  inactive  -
edgexfoundry.support-scheduler             disabled  inactive  -
edgexfoundry.sys-mgmt-agent                disabled  inactive  -
edgexfoundry.vault                         enabled   active    -
```

#### Configure edgexfoundry:

When we use Edgex UI work for streams, there is a problem of 502 Bad gateway. Because the Rest API of eKuiper in EdgeX uses port 59720 instead of the default 9081. So please change 9081 to 59720:

```
$ cd /var/snap/edgexfoundry/current/kuiper/etc
$ sudo nano kuiper.yaml
```

Chang: restPort: 9081 
To:     restPort: 59720

```
$ snap set edgexfoundry kuiper=on
```

#### Start Scheduler service:

Enable Scheduler, to use Interval and interval actions for cleaning up Redis database periodically:

```
$ snap set edgexfoundry support-scheduler=on 
$ snap set edgexfoundry support-notifications=on 
```

#### Start System Management Agent service:

Enable system management agent service for communication between SMA and core data, to use ‘system services monitor’ for system health check, metric check, configuration and operation:

```
$ snap set edgexfoundry sys-mgmt-agent=on
```

#### Use edgex-ui:

Open this in your brower: http://localhost:4000
We should see this:![img](https://lh3.googleusercontent.com/NhfgfQlvaCUxmtZo9Y1dbZpFN5vndNJ3fK4lVplLCVSSkgE7B8DkxguIlFF7LAgXrcEXy9amBJ7DE2QLTqBUkxTQCQnVtQk5mEZLeW8Td1EHjlMj7NVJkkoz65bAvqk6E7VGqcwu=s0)
