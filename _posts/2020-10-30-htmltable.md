---
layout: post
categories: posts
title: THML MOVIE TABLE
subtitle: This is a test post.
tags: [code post]
date-string: OCTOBER 30, 2020
---

```python
response = requests.get('https://movie.naver.com/movie/sdb/rank/rmovie.nhn')
html = response.text

soup = BeautifulSoup(html, 'html.parser')
@app.route('/')
def movie():
    diction=dict()
    ranking = 1
    for tag in soup.select('div[class=tit3]'):
        url = tag.get('href')
        diction[ranking]=tag.text.strip()
        ranking = ranking + 1
    return render_template('a.html', values = diction)
if __name__ == "__main__":
    app.run(host="127.0.0.1", port="8080")
```

<pre>
<html>
    <head>
    <style>
        table {
          font-family: arial, sans-serif;
          border-collapse: collapse;
          width: 100%;
        }
        td, th {
          border: 1px solid #dddddd;
          text-align: left;
          padding: 8px;
        }
        tr:nth-child(even) {
          background-color: #dddddd;
        }
    </style>
    </head>
    <body>
        <h2>HTML MOVIE TABLE</h2>
    <table>
      <tr>
        <th>순위</th>
        <th>영화</th>
      </tr>
      {% for key,value in values.items() %}
      <tr>
        <td>{{ key }} 순위</td>
        <td>{{ value }}</td>
      </tr>
      {% endfor %}
    </table>
    </body>
</html>
</pre>
