# gitbook-plugin-mouseflow
Add [mouseflow](https://mouseflow.com/) to gitbook web pages

### How to use?

Add plugin to your `book.json`, then run `gitbook install`:

```json
{
    "plugins": ["mouseflow"]
}
```

#### Configure mouseflow token:


```json
{
    "plugins": ["mouseflow"],
    "pluginsConfig": {
        "mouseflow": {
            "projectId": "12345678abc"
        }
    }
}
```

### Inspiration
Inspired by: https://github.com/chudaol/gitbook-plugin-gtm
