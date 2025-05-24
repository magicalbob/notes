Import dashboard from JSON
==========================

	Command:

		curl -X POST -H "Authorization: Bearer TOKEN" \
		     -H "Content-Type: application/json" \
		     -d '{"dashboard": '"$(cat dashboard_gitlab.json)"', "overwrite": true}' \
		     https://grafana.ellisbs.co.uk/api/dashboards/db

	Will output message like this for success:

		{
		  "folderUid": "",
		  "id": 49,
		  "slug": "gitlab",
		  "status": "success",
		  "uid": "be58a7fii0wsga",
		  "url": "/grafana/d/be58a7fii0wsga/gitlab",
		  "version": 1
		}
