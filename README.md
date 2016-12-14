datarobot-prime-server
====

HTTP API server for DataRobot Prime.


# Usage

- 1. name your DataRobot Prime python code `prime.py` and put it same location into `server.py`
- 2. run server.py

example:

```bash
# 1.
    $ cp your_model_DataRobot_Prime.py ./prime.py

# 2.
    $ python server.py
```

## HTTP API

### GET /status

Check server status.

- exmaple

```bash
$ curl http://localhost:8080/status

{"status": "ok", "code": 200, "error": null}
```

### POST /predict

Get prediction results by given parameters.  
Response is number array style and prediction results are shown in the `"predicts"` key of response.

- exmaple

```bash
# Single prediction

$ curl -XPOST http://localhost:8080/predict \
   -H 'Content-Type: application/json' \
   -d '{ "user_id": 100, "age": 20 }'

{"error": null, "code": 200, "predicts": [0.3727827380524341]}
```

```bash
# Multiple prediction

$ curl -XPOST http://localhost:8080/predict \
   -H 'Content-Type: application/json' \
   -d '[{ "user_id": 100, "age": 20 }, { "user_id": 200, "age": 35 }]'

{"error": null, "code": 200, "predicts": [0.3727827380524341, 0.26119423557818366]}
```

### POST /predict/[_key_]

Get prediction results by given parameters.  
Response is object array style which is created by the `<key>`.

- exmaple

```bash
# Single prediction

$ curl -XPOST http://localhost:8080/predict/user_id \
   -H 'Content-Type: application/json' \
   -d '{ "user_id": 100, "age": 20 }'

{"error": null, "code": 200, "data": [{"user_id": 100, "predict": 0.3727827380524341}]}
```

```bash
# Multiple prediction

$ curl -XPOST http://localhost:8080/predict \
   -H 'Content-Type: application/json' \
   -d '[{ "user_id": 100, "age": 20 }, { "user_id": 200, "age": 35 }]'

{"error": null, "code": 200, "data": [{"user_id": 100, "predict": 0.3727827380524341}, {"user_id": 200, "predict": 0.26119423557818366}]}
```


## Environment Variables

| env | description |
|:---|:---|
| DR_SERVER_PORT | HTTP port number for listening (default=8080) |


## For Python3

Some syntax might not be compatible to Python3 in the DataRobot Prime code.
If you want to use it with Python3 environment, try below change,

```bash
sed -e "s@re.findall(ur@re.findall(r@g" -i '' ./prime.py
```

## Docker

Check this out.

[datarobot](https://github.com/evalphobia/dockerfiles/tree/master/datarobot)
