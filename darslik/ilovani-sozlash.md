# Ilovani sozlash

Flask ilovasi Flask sinfining namunasidir. Ilovaga oid hamma narsa, masalan, konfiguratsiya va URL manzillar ushbu sinfda ro'yxatdan o'tkaziladi.&#x20;

Flask ilovasini yaratishning eng oddiy usuli bu global Flask nusxasini to'g'ridan-to'g'ri kodingizning yuqori qismida yaratishdir, masalan, oldingi sahifada misol qilingan "Hello World!" ilovasi. Bu ba'zi hollarda oddiy va foydali bo'lsa-da, loyiha o'sib borishi bilan ba'zi qiyin muammolarni keltirib chiqarishi mumkin.

Global miqyosda Flask misolini yaratish o'rniga, uni funksiya ichida yaratasiz. Bu funksiya dastur fabrikasi sifatida tanilgan. Ilovaga kerak bo'lgan har qanday konfiguratsiya, ro'yxatga olish va boshqa sozlashlar funksiya ichida amalga oshiriladi, so'ngra ilova qaytariladi.

## Ilova fabrikasi

Kodlashni boshlash vaqti keldi! flaskr katalogini yarating va \_\_init\_\_.py faylini qo'shing. \_\_init\_\_.py ikki tomonlama vazifani bajaradi: u ilovalar zavodini o'z ichiga oladi va u Pythonga flaskr katalogiga paket sifatida qarash kerakligini aytadi.

```bash
$ mkdir flaskr
```

{% code title="flaskr/__init__.py" %}
```python
import os

from flask import Flask


def create_app(test_config=None):
    # create and configure the app
    app = Flask(__name__, instance_relative_config=True)
    app.config.from_mapping(
        SECRET_KEY='dev',
        DATABASE=os.path.join(app.instance_path, 'flaskr.sqlite'),
    )

    if test_config is None:
        # load the instance config, if it exists, when not testing
        app.config.from_pyfile('config.py', silent=True)
    else:
        # load the test config if passed in
        app.config.from_mapping(test_config)

    # ensure the instance folder exists
    try:
        os.makedirs(app.instance_path)
    except OSError:
        pass

    # a simple page that says hello
    @app.route('/hello')
    def hello():
        return 'Hello, World!'

    return app
```
{% endcode %}

