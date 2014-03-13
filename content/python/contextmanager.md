Title: contextmanager && with in python
Date: 2014-03-11 12:14:00
Category: Python
Tags: python,contextmanager,with
Author: Cooper
Summary: contextmanager 的作用机制和理解

## Signal 的三种测试代码

1. `template_rendered.connect` + `contextmanager`

        :::python
        @contextmanager
        def captured_templates(app):
            recorded = []

            def record(sender, template, context, **extra):
                recorded.append((template, context))
            template_rendered.connect(record, app)
            try:
                yield recorded
            finally:
                template_rendered.disconnect(record, app)

        with captured_templates(app) as templates:
            rv = app.test_client().get('/')
            assert rv.status_code == 200
            assert len(templates) == 1
            template, context = templates[0]
            assert template.name == 'index.html'
            print len(context)

2. 无修饰符

        :::python
        recorded = []

        def record(sender, template, context, **extra):
            recorded.append((template, context))

        template_rendered.connect(record, app)
        rv = app.test_client().get('/')

        try:
            assert rv.status_code == 200
            assert len(recorded) == 1
            template, context = recorded[0]
            assert template.name == 'index.html'
            print len(context)
        finally:
            template_rendered.disconnect(record, app)

3. `template_rendered.connect_to` 在 `Blinker` 1.1 中引进

        :::python
        def captured_templates(app, recorded, **extra):
            def record(sender, template, context):
                recorded.append((template, context))
            return template_rendered.connected_to(record, app)
        templates = []
        with captured_templates(app, templates):
            app.test_client().get('/')
            template, context = templates.pop()
            assert template.name == 'index.html'
            print len(context)

            app.test_client().get('/index')
            template, context = templates.pop()
            assert template.name == 'index.html'
            print len(context)

