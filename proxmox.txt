To get pve ticket from proxmox:

	curl -k -H "Content-Type: application/json" \
	     -d '{"username":"root","password":"SECRET","realm":"pam"}' \
	     "https://node1:8006/api2/json/access/ticket"|jq .

Return includes something like:

	    "ticket": "PVE:root@pam:67A89C49::TePhqmoAapW5f/eLH6fnKv5hdRduBzFh/hvmM63ZDiRf9bIyCJdeIRXTDandEKNQY631/5rjR0kvX9Lnj66ZDfiKc18t696huAcL7F2TqAKRk9n0d7jAt512u45Z9U0TqDeEbAyw0xK9j3tJxTNE7gwWt4A5fUBcjXecHGH1G1GjW8/VsFFryP6FDDE52oJ3Vk8GJGvvIpmB22HyP8DPmn6ELcnHJMiEr/FjdI4PgX2gpLrU+Oc8BYZTsrqBwA7oAOz3buHyPJw+taLd07K1Ex/f4jHSD5uD6XGuDu+mMczuJsgA+OIIlJ5bFJIZu4Hk0fBTAqCk9SMXcjPhUntT2w=="

To use pve ticket to get VM details:

	curl -f -k -H "Authorization: PVEAuthCookie=PVE:root@pam:67A89C49::TePhqmoAapW5f/eLH6fnKv5hdRduBzFh/hvmM63ZDiRf9bIyCJdeIRXTDandEKNQY631/5rjR0kvX9Lnj66ZDfiKc18t696huAcL7F2TqAKRk9n0d7jAt512u45Z9U0TqDeEbAyw0xK9j3tJxTNE7gwWt4A5fUBcjXecHGH1G1GjW8/VsFFryP6FDDE52oJ3Vk8GJGvvIpmB22HyP8DPmn6ELcnHJMiEr/FjdI4PgX2gpLrU+Oc8BYZTsrqBwA7oAOz3buHyPJw+taLd07K1Ex/f4jHSD5uD6XGuDu+mMczuJsgA+OIIlJ5bFJIZu4Hk0fBTAqCk9SMXcjPhUntT2w==" \
	     "https://node1:8006/api2/json/nodes/node1/qemu"|jq .
	
	  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
	                                 Dload  Upload   Total   Spent    Left  Speed
	100   395  100   395    0     0   4716      0 --:--:-- --:--:-- --:--:--  4759
	{
	  "data": [
	    {
	      "cpu": 0,
	      "diskread": 0,
	      "name": "pm1",
	      "disk": 0,
	      "netin": 0,
	      "mem": 0,
	      "diskwrite": 0,
	      "status": "stopped",
	      "maxdisk": 34359738368,
	      "vmid": 201,
	      "maxmem": 2147483648,
	      "netout": 0,
	      "uptime": 0,
	      "cpus": 1
	    },
	    {
	      "name": "ubuntu-template",
	      "cpu": 0,
	      "diskread": 0,
	      "mem": 0,
	      "diskwrite": 0,
	      "status": "stopped",
	      "disk": 0,
	      "netin": 0,
	      "maxmem": 2147483648,
	      "netout": 0,
	      "template": 1,
	      "vmid": 100,
	      "maxdisk": 34359738368,
	      "cpus": 1,
	      "uptime": 0
	    }
	  ]
	}
