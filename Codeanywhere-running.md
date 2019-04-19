

```python
from datetime import datetime
import feedparser
from flask import Flask
app = Flask(__name__)
cache = {}
time = datetime.now()

@app.route("/")
def home():
    if 'html' not in cache or (datetime.now()-cache['html'][1]).seconds>120:
        html = gethtml()
        cache['html'] = (html,datetime.now())
    else:
        html = cache['html'][0]
    return html
  
def gethtml():
    feed = feedparser.parse("https://9gag-rss.com/api/rss/get?code=9GAGAwesome&format=2")
    title = feed['feed']['title']
    entries = feed.entries

    html = '''<!DOCTYPE html><html><head><meta charset="utf-8"><title>''' + title + '''</title></head><body>'''
    html += '''<table border="1"><tr><th>title</th><th>summary</th><th>author</th><th>updated</th><th>link</th></tr>'''
    for entry in entries:
        summary = entry.summary
        html += '''<tr>'''
        html += '''<td>''' + entry.title + '''</td>'''
        html += '''<td>''' + summary + '''</td>'''
        html += '''<td>''' + entry.author + '''</td>'''
        html += '''<td>''' + entry.updated + '''</td>'''
        html += '''<td>''' + entry.link + '''</td>'''
        html += '''</tr>'''
    html += '''</body></html>'''
    return html
if __name__ == '__main__':
    app.run(host='0.0.0.0', port = 3000)
```
