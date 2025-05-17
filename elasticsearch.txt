List indices:

	curl http://dev.ellisbs.co.uk:9200/_cat/indices/

Reduce replica count to zero:

	curl -X PUT "http://dev.ellisbs.co.uk:9200/metricbeat-7.17.26-2025.02.11-000002/_settings" -H 'Content-Type: application/json' -d'
{
  "index": {
    "number_of_replicas": 0
  }
}'

Force allocation of the shards (if there are other issues)

	If needed, you can try to manually reroute shards:

		# Find specific unassigned shards first
		curl -X GET "http://dev.ellisbs.co.uk:9200/_cat/shards?v" | grep UNASSIGNED

		# Then force allocation (example - adjust shard number as needed)
		curl -X POST "http://dev.ellisbs.co.uk:9200/_cluster/reroute" -H 'Content-Type: application/json' -d'
		{
		  "commands": [
		    {
		      "allocate_empty_primary": {
		        "index": "metricbeat-7.17.26-2025.02.11-000002", 
		        "shard": 0,
		        "node": "node_name",
		        "accept_data_loss": true
		      }
		    }
		  ]
		}'

Sample documents from an index

		# Get 10 documents from a specific index
		curl -X GET "http://dev.ellisbs.co.uk:9200/metricbeat-7.17.26-2025.02.11-000002/_search?pretty" -H 'Content-Type: application/json' -d'
		{
		  "size": 10,
		  "query": {
		    "match_all": {}
		  }
		}'
