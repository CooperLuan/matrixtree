Title: Flask Notes
Date: 2014-03-11 00:30:00
Category: Python
Tags: python,flask
Author: Cooper
Summary: Flask 学习笔记

## 目录结构

`Flask` 的目录结构同 `tornado`

    :::
    /flaskr
        /static
        /templates


+ `static` 静态资源 css/js 用 /static/ 访问
+ `templates` # 默认模板目录

## 基本 module

    :::python
    from flask import Flask, request, session, g, redirect, url_for, abort, \
     render_template, flash, jsonify

+ `Flask`
+ `request` 封装的请求对象 `requests.args.get('id')`
+ `session` 同 `request`
+ `g` 全局变量
+ `redirect` 重定向对象
+ `url_for` 用于生成静态资源 url
+ `abort` 抛出错误 4**
+ `render_template` 模板渲染
+ `flash` 设置变量供下一次请求使用
+ `jsonify` 将 `json` 对象转化为 `Response`

## 基本配置

    :::python
    # create our little application :)
    app = Flask(__name__)
    app.config.from_object(__name__)
    # Load default config and override config from an environment variable
    app.config.update(dict(
        DATABASE=os.path.join(app.root_path, 'flaskr.db'),
        DEBUG=True,
        SECRET_KEY='development key',
        USERNAME='admin',
        PASSWORD='default'
    ))
    app.config.from_envvar('FLASKR_SETTINGS', silent=True)
    def connect_db():
        """Connects to the specific database."""
        rv = sqlite3.connect(app.config['DATABASE'])
        rv.row_factory = sqlite3.Row
        return rv
    def get_db():
        if not hasattr(g, 'sqlite_db'):
            g.sqlite_db = connect_db()
        return g.sqlite_db
    if __name__ == '__main__':
        app.run()

详细的配置处理见 [Configuration Handling](http://flask.pocoo.org/docs/config/)

## 简单请求处理 && session && flash

    :::python
    @app.route('/')
    def index():
        if not session.get('logged_in'):
            abort(401)
        return 'hello tank'
    @app.route('/login', methods=['POST'])
    def login():
        error = None
        if request.method == 'POST':
            if request.form['username'] != app.config['USERNAME']:
                error = 'Invalid username'
            elif request.form['password'] != app.config['PASSWORD']:
                error = 'Invalid password'
            else:
                session['logged_in'] = True
                flash('You were logged in')
                return redirect(url_for('show_entries'))
        return render_template('login.html', error=error)
    @app.route('/logout')
    def logout():
        session.pop('logged_in', None)
        flash('You were logged out')
        return redirect(url_for('show_entries'))

`flash` 可以用于下一次请求

## 模板

`flask` 默认用了 `jinja2` 的模板引擎，这点比 `tornado` 好，避免模板引擎不够用的时候重复造轮子

`tornado` 的模板是由 `header/body/footer` 等几个 `modules` 组成的；而 `jinja2` 由 `base.html` 定义好结构(分类)，再在目标模板中重写/覆盖需要的模块

## 返回 json

    :::python
    @app.route('/status')
    def status():
        return jsonify

## 错误处理

[Logging Application Errors](http://flask.pocoo.org/docs/errorhandling/)

## 其它特性

+ [Signals](http://flask.pocoo.org/docs/signals/)
+ [Pluggable Views](http://flask.pocoo.org/docs/views/)

## Schedule

+ `2014-03-11 01:11:00` [The Application Context](http://flask.pocoo.org/docs/appcontext/)
