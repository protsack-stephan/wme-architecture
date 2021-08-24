# Firehose service MVP


## Articles
`GET /v2/articles` - endpoint to get all article related events. We can than add filtering to this for example (`/v2/articles?project=enwiki&namespace=14` or `/v2/articles?language=en&namespace=14`)
```json
{
   "event": {
    "identifier": "01ae1f13-f6f5-4e16-ac44-71e24f3d85ab",
    "type": "create", // type wil change (create, update, delete)
    "date_created": "2021-07-12T15:11:57.692671588Z"
  },
  "identifier": 100,
  "name": "Earth"
  // ...other fields go here...
}
```

`GET /v2/article-visibility` - endpoint to get live stream of article visibility change events.
```json
{
   "event": {
    "identifier": "01ae1f21-f6f5-4e16-ac44-71e24f3d85ab",
    "type": "visibility-change",
    "date_created": "2021-07-12T15:11:57.692671588Z"
  },
  "identifier": 100,
  "name": "Earth"
  // ...other fields go here...
}
```



## Entities
`GET /v2/entities` - live stream of all wikidata changes.
```json
{
   "event": {
    "identifier": "01ae1f13-f6f5-4a12-ac44-71e24f3d85ab",
    "type": "create", // type wil change (create, update, delete)
    "date_created": "2021-07-12T15:11:57.692671588Z"
  },
  "identifier": "Q2",
  "name": "Earth"
  // ...other fields go here...
}
```

`GET /v2/entity-visibility` - endpoint to get live stream of entity visibility change events.
```json
{
   "event": {
    "identifier": "01ae1f13-f6f5-4a12-ac44-71e24f3d85ab",
    "type": "create", // type wil change (create, update, delete)
    "date_created": "2021-07-12T15:11:57.692671588Z"
  },
  "identifier": "Q2",
  "name": "Earth"
  // ...other fields go here...
}
```
## Advanced stuff (from the top of my head, needs more thinking)

`POST /v2/streams` Create a stream with a certain conditions and joins example payload (again needs more design).

```json
# example request
{
  "source": "articles",
  "join": [
    "version",
    "entity"
  ],
  "conditions": {
    "and": [
        {
          "field": "project",
          "value": "enwiki"
        },
        {
          "field": "language",
          "value": "en"
        }
      ],
      "or": {
        {
          "field": "namespace",
          "value": 0
        },
        {
          "field": "namespace",
          "value": 14
        }
      }
  }
}
```

```JSON
# example response
{
  "identifier": "57a7c968-bf3f-4ef1-b191-f1f64682a657"
}
```

`GET /v2/streams/{identifier}` - connect to the stream you have created (in our example `/v2/streams/articles/57a7c968-bf3f-4ef1-b191-f1f64682a657`).

```json
{
   "event": {
    "identifier": "01ae1f13-f6f5-4e16-ac44-71e24f3d85ab",
    "type": "create", // type wil change (create, update, delete)
    "date_created": "2021-07-12T15:11:57.692671588Z"
  },
  "identifier": 100,
  "name": "Earth",
  "version": {
    // ...version data... (showing this here as an example of join)
  },
  "entity": {
    // ...entity data... (showing this here as an example of join)
  }
  // ...other fields go here...
}
```

`DELETE /v2/streams/{identifier}` - delete the stream.

`PUT /v2/streams/{identifier}` - update the stream.

