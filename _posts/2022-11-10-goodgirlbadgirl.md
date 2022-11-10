---
layout: post
categories: posts
title: News positive/negative analysis
subtitle: let's read the news
tags: [code post]
date-string: NOVEMBER 10, 2022
---
```python
from konlpy.tag import Kkma
import requests, json
url = requests.get("https://raw.githubusercontent.com/park1200656/KnuSentiLex/master/data/SentiWord_info.json")
text = url.text
data = json.loads(text)
lists=[]
for n in data:
    dic = {}
    dic['word_root'] = n['word_root']
    dic['polarity'] = n['polarity']
    lists.append(dic)
dt = {i['word_root']:i for i in lists}.values()
kkma = Kkma()
# dt = list(map(dict, set(tuple(sorted(d.items())) for d in lists)))
def kkk(popo):
    sds=0
    di={}
    morph = kkma.morphs(popo)
    for n in morph:
        for s in dt:
            if n == s['word_root']:
                sd = int(s['polarity'])
                di[n]=sd
                sds = sd + sds
            else:
                pass
    list=[di,sds]
    return list
```

> 뉴스 기사를 가져와서 긍정 부정 계산하기

```python
import re
import requests
from bs4 import BeautifulSoup

def collect(url):
    headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) Chrome/98.0.4758.102"}
    url=url
    req = requests.get(url, headers=headers)
    html = req.text
    soup = BeautifulSoup(html,'html.parser')
    pattern1='<[^>]*>'
    content = soup.select("div#dic_area")
    content = ''.join(str(content))
    content = re.sub(pattern=pattern1, repl='',string=content)
    content = content.replace("\n","")
    pattern2 ="""[\n\n\n\n\n// flash 오류를 우회하기 위한 함수 추가\nfunction _flash_removeCallback() {}"""
    content=content.replace(pattern2,'')
    return content
```

> 뉴스 기사 본문을 가져오는 함수

```python
from flask import Flask, render_template, request, url_for, redirect
import pymysql.cursors
from happybadword import kkk
from sdsd import collect
app = Flask(__name__)

@app.route('/submit/<code>', methods = ['GET'])
def info(code):
    conn = pymysql.connect(host='',
                           user='',
                           password='',
                           db='',
                           charset='utf8')
    try:
        with conn.cursor() as cursor:
            sql="SELECT rink FROM news WHERE rink LIKE '%%%s%%'" %code
            cursor.execute(sql)
            result = cursor.fetchone()
    finally:
        conn.close()
    url = result[0]
    a=collect(url)
    print(result)
    print(a)
    kkk('')
    di = kkk(a)
    sds=di[1]
    di = di[0]
    print(di)
    if request.method =='GET':
        return render_template('info.html', di =di,sds=sds, result = result,a=a)
@app.route('/')
def student():
   return render_template('start.html')
@app.route('/result',methods = ['POST', 'GET'])
def result():
    conn = pymysql.connect(host='',
                           user='',
                           password='',
                           db='',
                           charset='utf8')
    #https://n.news.naver.com/mnews/article/016/0002025287?sid=105
    if request.method == 'POST':
        result = request.form
        # print(result)
        try:
            with conn.cursor() as cursor:
                sql = 'SELECT * FROM news WHERE date = "%s" AND categorie = "%s"' %(result['Date'], result['Categorie'])
                cursor.execute(sql)
                # print(cursor.fetchall())
                result = cursor.fetchall()
        finally:
            conn.close()
        # print(result)
        s = result[1][1].split('/')
        # print(s[6][:-8])
        ls=[]
        for n in result:
            l=[]
            s = n[1].split('/')
            for code in n:
                l.append(code)
            l[1] = s[6][:-8]
            ls.append(l)
        print(ls)
        return render_template("result.html", ls = ls)
if __name__ == '__main__':
   app.run(debug = True)
from flask import Flask, render_template, request, url_for, redirect
import pymysql.cursors
from happybadword import kkk
from sdsd import collect
app = Flask(__name__)

@app.route('/submit/<code>', methods = ['GET'])
def info(code):
    conn = pymysql.connect(host='',
                           user='',
                           password='',
                           db='',
                           charset='utf8')
    try:
        with conn.cursor() as cursor:
            sql="SELECT rink FROM news WHERE rink LIKE '%%%s%%'" %code
            cursor.execute(sql)
            result = cursor.fetchone()
    finally:
        conn.close()
    url = result[0]
    a=collect(url)
    print(result)
    print(a)
    kkk('')
    di = kkk(a)
    sds=di[1]
    di = di[0]
    print(di)
    if request.method =='GET':
        return render_template('info.html', di =di,sds=sds, result = result,a=a)
@app.route('/')
def student():
   return render_template('start.html')
@app.route('/result',methods = ['POST', 'GET'])
def result():
    conn = pymysql.connect(host='',
                           user='',
                           password='',
                           db='',
                           charset='utf8')
    #https://n.news.naver.com/mnews/article/016/0002025287?sid=105
    if request.method == 'POST':
        result = request.form
        # print(result)
        try:
            with conn.cursor() as cursor:
                sql = 'SELECT * FROM news WHERE date = "%s" AND categorie = "%s"' %(result['Date'], result['Categorie'])
                cursor.execute(sql)
                # print(cursor.fetchall())
                result = cursor.fetchall()
        finally:
            conn.close()
        # print(result)
        s = result[1][1].split('/')
        # print(s[6][:-8])
        ls=[]
        for n in result:
            l=[]
            s = n[1].split('/')
            for code in n:
                l.append(code)
            l[1] = s[6][:-8]
            ls.append(l)
        print(ls)
        return render_template("result.html", ls = ls)
if __name__ == '__main__':
   app.run(debug = True)
```
> 플라스크를 활용하여 뉴스 목록을 보여주고 선택한 뉴스의 긍/부정과 본문 보여주기
