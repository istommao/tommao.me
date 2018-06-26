---
date: 2016-12-13 14:08
status: public
title: Python的web框架sanic
---

> Python 3.5+ web server that's written to go fast

`前段时间发现了一个新的Python web框架，看到Benchmarks有点心动，决定上手实验一波`

![](~/14-09-58.jpg)

```python
from sanic import Sanic
from sanic.response import html

from jinja2 import Environment, PackageLoader

env = Environment(loader=PackageLoader('app', 'templates'))

app = Sanic(__name__)

@app.route('/')
async def test(request):
    data = {'name': 'Hello world!'}

    template = env.get_template('index.html')
    html_content = template.render(name=data["name"])
    return html(html_content)

app.run(host="127.0.0.1", port=8000)
```
`文件目录`

![](~/14-12-14.jpg)

![](~/14-11-44.jpg)
