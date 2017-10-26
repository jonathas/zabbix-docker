# Zabbix Monitoring Server

These files are for running the Zabbix server and the Zabbix agent on Docker.

## Zabbix Server

Change the password inside the docker-compose.yml file, as the one that is there is a random one.

Start the server as root or with sudo, as the agent needs to be privileged:

```bash
$ sudo docker-compose up -d
```

For the first time you login, the credentials are:

Username: Admin
Password: zabbix

## Zabbix Agent

In the servers you want to monitor, you need to install a Zabbix agent which communicates with the Zabbix server. This agent needs to run in privileged mode, so it is able to get deeper system statistics.

1 - Send the content inside the zabbix-agent directory to the server you want to monitor, then you can start it with:

```bash
$ sudo docker-compose up -d
```

2 - Change the path inside the zabbix-agent/host/etc/systemd/system/docker-zabbix-agent.service to the one where your docker-compose.yml file is in the server.

3 - Copy this docker-zabbix-agent.service you just changed to the directory /etc/systemd/system

4 - Reload the daemons and enable the one you just created, so it runs automatically in case the system is rebooted:

```bash
$ sudo systemctl daemon-reload
$ sudo systemctl enable docker-zabbix-agent
```

In the Zabbix web interface, go to Configuration => Hosts, then fill the name of your host in "Host name" field, add it to the "Linux servers" group, put your server's IP address in the "Agent interfaces" section and be sure to have the "Enabled" checkbox checked.

![Zabbix add host](/zabbix_add_host.png "Zabbix add host")

After this you can go to Monitoring => Screens and configure your server monitoring dashboard there.
