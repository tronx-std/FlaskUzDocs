# Loyiha tuzilishi

Loyiha uchun papka yarating va ushbu kodni kiriting:

```bash
$ mkdir flask-tutorial
$ cd flask-tutorial
```

Virtual muhit yaratish va loyihaga flaskni o'rnatish uchun [ornatish.md](../ornatish.md "mention") mavzusini o'qing.

***

Flask ilovasi bitta fayl kabi oddiy bo'lishi mumkin.

{% code title="hello.py" %}
```python
from flask import Flask

app = Flask(__name__)


@app.route('/')
def hello():
    return 'Hello, World!'
```
{% endcode %}

Biroq, loyiha kattalashgani sayin, barcha kodlarni bitta faylda saqlash juda qiyin bo'ladi. Python loyihalari kodni bir nechta modullarga tartibga solish uchun paketlardan foydalanadi, ularni kerak bo'lganda import qilish mumkin va qo'llanma ham shunday qiladi.

Loyiha katalogida quyidagilar bo'ladi:

* `flaskr/`, sizning loyihangiz kodlarini o'z ichiga oladigan paket.
*
