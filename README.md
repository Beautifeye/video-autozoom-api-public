# Autozoom - API Documentation


## Introduction


Welcome to the Sparkd Videos Autozoom API! You can use our API to access Sparkd Videos API endpoints, which will analyse any input video according to the API's specifications. 

Last updated version of the API includes the following modules:

- [Upload video](#autozoom)
- [Results API](#polling-api)

## Authentication

Sparkd Videos uses API keys to allow access to the API.
You can register a new Sparkd Video API key getting in contact with our team at: [info@sparkd.ai](mailto:info@sparkd.ai)

## Upload video

Ths API request performs automatic sport events autozoom of the input video. The API will analyse the input video in order to create a virtual moving camera that follows the gameplay, just like a professional cameraman would do.

### HTTP Request

`POST  http://ec2-54-154-28-171.eu-west-1.compute.amazonaws.com:9001/api/autozoom/upload`


### Header

|   Key	                    |Value 	                  | Description   	|
|---	                    |---	                      |---	            |
|   Authorization	        | "Token \<YOUR_TOKEN\>"         |   	Authorization token. You have at most ``n`` permitted request per month.           |

### URI Parameters

|   Parameter	        | Default 	                  | Description   	|
|---	                |---	                      |---	            |
|   video_url	        | (required)                  |   	(str) Public link to the video to be analysed.           |
|   id	                | (required)                  |   	 (str)     Unique id related to the video on the Sparkd Videos Database. It has to be a randomly-generated version 4 UUID.   |
|   game_id	            | (required)  	              |   	 (str)    Unique id related to the video for external uses.   |
|   sport	            | (optional) ``"soccer"``     |   (str)	Name of the sport peformed into the video. Possible values are: ``"soccer"``, ``"futsal"``, ``"basket"``          |
|   config_file      	|  (optional)	``{}``        | (dict) It is a set of paramters that allows to optimize the algorithm results to the specifc camera and scene recorded.<br>                                                                                      If empty, a default set of parameters will be used.	            |
|   ack_url	                |  (optional) 	              | (str) If used, you will receive a json with the ``id`` and ``game_id`` at this endpoint when the results are ready to be pulled.	|
|   is_testing_flow	    | (optional) ``false``        | (bool) Select the proper ML pipeline to trigger: production or testing. Set ``true`` only for development activities.  	|


## Results API

This API requests search for any information regarding the video ``id`` on the Sparkd Videos Database and, if something is retrieved, it allows you to get the related data.

### HTTP Request

`GET  http://ec2-54-154-28-171.eu-west-1.compute.amazonaws.com:9001/api/autozoom/results/<id>`

### Header

|   Key	                    |Value 	                  | Description   	|
|---	                    |---	                      |---	            |
|   Authorization	        | "Token \<YOUR_TOKEN\>"         |   	Authorization token. You will not be charged for these API requests since no ML pipeline will be triggered.      |


### Parameters

|   Parameter	        | Default 	                  | Description   	|
|---	                |---	                      |---	            |
|   id	                | (required)                  |   	 (str)     Unique id related to the video on the Sparkd Videos Database. It has to be the same randomly-generated version 4 UUID you used to trigger the ML pipeline.   |


### Output format

The operation will return a JSON file with the following items:

|   Key	        | Description   	|
|---	                |---	            |
|   id	                |   	 (str)     Unique id related to the video on the Sparkd Videos Database. It will be the same randomly-generated version 4 UUID you used to trigger the endpoint.   |
|   results	                |   	 (list)     List of maps containing the autozoom results. Each element will have 4 keys: <ul><li>"ts": timestep which the coordinates are related to.</li><li>"x": relative position [0,1] on the x-axis of the center of the virtual frame.</li><li>"y": relative position [0,1] on the y-axis of the center of the virtual frame</li><li>"z": relative zoom on the original frame size.</ul>   |

Example of results:<br>
```yaml
{
"id": "a126b358-3c1b-4f83-8dee-f037e5ca6880",
"results": [{"ts":0.0, "x":0.5, "y":0.5, "z":6.1}, {"ts":0.1, "x":0.5, "y":0.6, "z":6.2}]
}
