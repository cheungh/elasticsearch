### Various ES server administration tasks -- Postman collection  
<pre>
{
	"variables": [],
	"info": {
		"name": "elastic",
		"_postman_id": "9163998b-5fec-b0b3-97d8-2721d6a23d00",
		"description": "",
		"schema": "https://schema.getpostman.com/json/collection/v2.0.0/collection.json"
	},
	"item": [
		{
			"name": "server",
			"description": "",
			"item": [
				{
					"name": "nodes",
					"request": {
						"url": {
							"raw": "http://localhost:9200/_nodes?pretty",
							"protocol": "http",
							"host": [
								"localhost"
							],
							"port": "9200",
							"path": [
								"_nodes"
							],
							"query": [
								{
									"key": "pretty",
									"value": "",
									"equals": false,
									"description": ""
								}
							],
							"variable": []
						},
						"method": "GET",
						"header": [],
						"body": {},
						"description": ""
					},
					"response": []
				},
				{
					"name": "nodes filter",
					"request": {
						"url": "http://localhost:9200/_nodes/os,jvm,thread_pool",
						"method": "GET",
						"header": [],
						"body": {},
						"description": ""
					},
					"response": []
				},
				{
					"name": "nodes stats",
					"request": {
						"url": "http://localhost:9200/_nodes/stats",
						"method": "GET",
						"header": [],
						"body": {},
						"description": ""
					},
					"response": []
				},
				{
					"name": "nodes stats filter",
					"request": {
						"url": "http://localhost:9200/_nodes/stats/jvm,os,breaker",
						"method": "GET",
						"header": [],
						"body": {},
						"description": ""
					},
					"response": []
				},
				{
					"name": "cluster",
					"request": {
						"url": {
							"raw": "http://localhost:9200/_cluster/health?pretty",
							"protocol": "http",
							"host": [
								"localhost"
							],
							"port": "9200",
							"path": [
								"_cluster",
								"health"
							],
							"query": [
								{
									"key": "pretty",
									"value": "",
									"equals": false,
									"description": ""
								}
							],
							"variable": []
						},
						"method": "GET",
						"header": [],
						"body": {},
						"description": ""
					},
					"response": []
				},
				{
					"name": "pending_tasks",
					"request": {
						"url": "http://localhost:9200/_cluster/pending_tasks",
						"method": "GET",
						"header": [],
						"body": {},
						"description": ""
					},
					"response": []
				},
				{
					"name": "tasks managament",
					"request": {
						"url": "http://localhost:9200/_tasks",
						"method": "GET",
						"header": [],
						"body": {},
						"description": ""
					},
					"response": []
				},
				{
					"name": "hot threads",
					"request": {
						"url": "http://localhost:9200/_nodes/hot_threads",
						"method": "GET",
						"header": [],
						"body": {},
						"description": ""
					},
					"response": []
				},
				{
					"name": "shard allocation",
					"request": {
						"url": "http://localhost:9200/_cluster/allocation/explain",
						"method": "GET",
						"header": [],
						"body": {},
						"description": ""
					},
					"response": []
				},
				{
					"name": "segment",
					"request": {
						"url": "http://localhost:9200/shakespeare/_segments",
						"method": "GET",
						"header": [],
						"body": {},
						"description": ""
					},
					"response": []
				},
				{
					"name": "cluster_state",
					"request": {
						"url": "http://localhost:9200/_cluster/state",
						"method": "GET",
						"header": [],
						"body": {},
						"description": ""
					},
					"response": []
				},
				{
					"name": "cache clear",
					"request": {
						"url": "http://localhost:9200/shakespeare/_cache/clear\n",
						"method": "POST",
						"header": [],
						"body": {},
						"description": ""
					},
					"response": []
				}
			]
		},
		{
			"name": "server copy",
			"description": "",
			"item": [
				{
					"name": "elasticsearch",
					"request": {
						"url": "",
						"method": "GET",
						"header": [],
						"body": {},
						"description": ""
					},
					"response": []
				},
				{
					"name": "cluster",
					"request": {
						"url": {
							"raw": "http://localhost:9200/_cluster/health?pretty",
							"protocol": "http",
							"host": [
								"localhost"
							],
							"port": "9200",
							"path": [
								"_cluster",
								"health"
							],
							"query": [
								{
									"key": "pretty",
									"value": "",
									"equals": false,
									"description": ""
								}
							],
							"variable": []
						},
						"method": "GET",
						"header": [],
						"body": {},
						"description": ""
					},
					"response": []
				},
				{
					"name": "pending_tasks",
					"request": {
						"url": "http://localhost:9200/_cluster/pending_tasks",
						"method": "GET",
						"header": [],
						"body": {},
						"description": ""
					},
					"response": []
				}
			]
		}
	]
}

</pre>
