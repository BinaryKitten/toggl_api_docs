Tasks
====================
Tasks are available only for pro workspaces.

Task has the following properties
* name: The name of the task (string, required, unique in project)
* pid: project ID for the task (integer, required)
* wid: workspace ID, where the task will be saved (integer, project's workspace id is used when not supplied)
* uid: user ID, to whom the task is assigned to (integer, not required)
* estimated_seconds: estimated duration of task in seconds (integer, not required)
* active: whether the task is active or not (boolean, by default true)
* at: timestamp that is sent in the response for PUT, indicates the time task was last updated

###Additional fields###
It's possible to get additional info for the task. For that you have to send the `fields` parameter in request with desired property names separated by comma.

* done_seconds: duration (in seconds) of all the time entries registered for this task
* uname: full name of the person to whom the task is assigned to

##Create a task##

`POST https://www.toggl.com/api/v8/tasks`

Example request

```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-type: application/json" \
	-d '{"task":{"name":"A new task","pid":777}}' \
	-X POST https://www.toggl.com/api/v8/tasks

```

Successful response
```json
{
	"data": {
		"name":"A new task",
		"id":1335076912,
		"wid":888,
		"pid":777,
		"active":true,
		"estimated_seconds":0
	}
}


```

##Update a task##

`PUT https://www.toggl.com/api/v8/tasks/{task_id}`

Workspace id (wid) and project id (pid) can't be changed.

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-H "Content-type: application/json" \
	-d '{"task":{"active":false,"estimated_seconds":3600,"fields":"done_seconds,uname"}}' \
	-X PUT https://www.toggl.com/api/v8/tasks/1335076912
```

Successful response
```json
{
	"data": {
		"name":"A new task",
		"id":1335076912,
		"wid":888,
		"pid":777,
		"active":false,
		"at":"2013-02-26T15:09:52+00:00",
		"estimated_seconds":3600,
		"uname": "John Swift",
		"done_seconds": 1200
	}
}
```

##Delete a task##

`DELETE https://www.toggl.com/api/v8/tasks/{task_id}`

Example request
```shell
curl -v -u 1971800d4d82861d8f2c1651fea4d212:api_token \
	-X DELETE https://www.toggl.com/api/v8/tasks/1335076912
```

Successful request will return `200 OK`. If the user has no access to delete, you'll get a status code `4xx`