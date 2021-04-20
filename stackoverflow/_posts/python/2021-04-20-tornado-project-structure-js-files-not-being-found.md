---
layout: post
title: Tornado Project Structure - Js files not being found
image: https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSxsuNgA2_J9lRj3yQD1tdwrL1ASkY1RjE_UapdJfULArLigerArg0rST0qFbWmAnRZ9Qw&usqp=CAU
description: >
  I need to set up a project which contains HTML, CSS, and JS files. Currently, my structure
hide_description: true
hide_image: true
comments: true
last_modified_at: 2021-04-20
categories: [stackoverflow]
tags: [python, tornado]
---

Tornado Project Structure - Js files not being found

{:.no_toc}
1. this unordered seed list will be replaced by toc as unordered list
{:toc}

## [Question](https://stackoverflow.com/questions/62286276/tornado-project-structure-js-files-not-being-found) by [HackleSaw](https://stackoverflow.com/users/11785644/hacklesaw)

I need to set up a project which contains HTML, CSS, and JS files. Currently, my structure is:

**project/:** 

 - static (css and js files)
 - templates (html)
app.py (Where the server starts)

But When I run app.py i.e
```py
import tornado.ioloop
import tornado.web

class MainHandler(tornado.web.RequestHandler):
    def get(self):
        # self.write("Hello, world")
        self.render("/full/path/to/file.html")

if __name__ == "__main__":
    application = tornado.web.Application([
        (r"/", MainHandler),
    ])
    application.listen(8050)
    tornado.ioloop.IOLoop.current().start()
```
I get this:

>WARNING:tornado.access:404 GET /static/style.css (127.0.0.1) 

>WARNING:tornado.access:404 GET /static/scriptfile.js (127.0.0.1) 

I'm not sure what am I doing wrong. I even changed the relative path to full paths of the JS and CSS files.


## [Answer](https://stackoverflow.com/a/65655346/13155046) by [Milovan Tomašević](https://stackoverflow.com/users/13155046/milovan-tomašević)

Tornado Project Structure :

```sh
project
├── app.py
├── common
│   ├── db_manip.py
│   └── utils.py
├── handlers
│   ├── base_handler.py
│   ├── news_handlers.py
│   └── user_handlers.py
├── static
│   ├── favicon.ico
│   ├── images
│   │   ├── favicon.ico
│   │   └── logo1.png
│   └── index.js
├── templates
│   ├── index.html
│   ├── login.html
│   ├── main.html
│   ├── profile.html
│   ├── signin.html
│   └── sources.html
└── requirements.txt
```

Ok, since we have _static_ in _template_ folders in the _root_ then:
1. Define `static_path` and `template_path` in your settings, it is directory from which static files will be served:

```py
settings = {
    "template_path": os.path.join(os.path.dirname(__file__), "templates"),
    "static_path": os.path.join(os.path.dirname(__file__), "static"),
    ...

}
```

2. Provide this settings to your application:

```py
app = tornado.web.Application(settings=settings, **kwargs)
```

3. Use `static_url` for your JS files in templates (see [`RequestHandler.static_url`][1]):

{% raw %}
```js
<script src="{{ static_url('index.js') }}" type="text/javascript"></script>
```
{% endraw %}

This is what a complete `main` would look like:

```py
if __name__ == '__main__':
    tornado.options.parse_command_line()

    settings = {
        "template_path": os.path.join(os.path.dirname(__file__), "templates"),
        "static_path": os.path.join(os.path.dirname(__file__), "static"),
        "cookie_secret": "_GENERATE_YOUR_OWN_RANDOM_VALUE_HERE__",
        "login_url": "/login",
        "db": db,
        "debug": True,
        "xsrf_cookies": True
    }

    app = tornado.web.Application(
        handlers=[(r'/', MainHandler),
                  (r'/sources', SourceHandler),
                  (r'/login', LoginHandler),
                  (r'/signin', SigninHandler),
                  (r'/profile', ProfileHandler),
                  (r'/favicon.ico', tornado.web.StaticFileHandler, dict(path=settings['static_path'])),
                  ],
        **settings
    )

    http_server = tornado.httpserver.HTTPServer(app)
    http_server.listen(options.port)
    try:
        tornado.ioloop.IOLoop.instance().start()
    except KeyboardInterrupt:
        print('Server has shut down.')

```

The same goes for `css` files.


  [1]: https://www.tornadoweb.org/en/latest/web.html#tornado.web.RequestHandler.static_url