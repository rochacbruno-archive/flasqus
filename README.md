# flasqus
A commenting system extension for Flask, threaded and API based working just like disqus but self hosted. 


## Why not disqus?

You can just use disqus, it is simple, scalable and has a lot of features but:
- Privacy
- Authentication (your readers needs to auth in disqus)
- Widgets (they have drop widgets so you need to use the API to get latest posts)
- Control (sometimes you need to have a better control of comments, rattings and sharings)

## How it works?

**Flasqus** is a Flask Extension, exposing some comment related blueprints and it is easy to customize and integrate with your existing Flask application.

You can attach Flasqus in your existing app just setting database and url configs or you can host standalone Flasqus application to consume throught the API.

## How to install it?

``pip install flasqus``

and then

```python
from flask import Flask
from flasqus import Flasqus
from flasqus.backends.mongo import FlasqusMongoEngine
# or
from flasqus.backends.sqlalchemy import FlasqusSQLAlchemy

app = Flask(..)
app.config['FLASQUS_MONGO_CONN'] = 'localhost:27017' # or app.config['FLASQUS_SQL_CONN'] = 'mysql://....'
app.config['FLASQUS_DATABASE'] = 'myappdb'
app.config['FLASQUS_COLLECTION'] = 'flasqus' # or app.config['FLASQUS_TABLE'] = 'flasqus'
app.config['FLASQUS_ENDPOINT'] = 'flasqus'
app.config['FLASQUS_URL_BASE'] = '/comments'
app.config['FLASQUS_AUTH_SYSTEM'] = 'flask_security'

Flasqus(app, backend=FlasqusMongoEngine)
```

With the above your app now will have the comment application mapped to **/comments** url and you will be able to add the following script to your post template in order to show the comment widget.

```javascript
<div id="flasqus_thread"></div>
<script type="text/javascript">
    /* * * CONFIGURATION VARIABLES * * */
    // you can handle different shortnames in the same app
    var flasqus_shortname = 'myappname';
    /* * * DON'T EDIT BELOW THIS LINE * * */
    (function() {
        var fsq = document.createElement('script'); fsq.type = 'text/javascript'; fsq.async = true;
        fsq.src = '//comments/static/embed.js';
        (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(fsq);
    })();
</script>
<noscript>Please enable JavaScript. comments powered by Flasqus</noscript>
```

Or you can forget javaScript and user Jinja Global to add

```html
<html>
   <h1> {{post.title}}</h2>
   <p> {{post.body|safe}}</p>
   
   <h2>Comments</h2>
   {{flasqus()}}
</html>
```






