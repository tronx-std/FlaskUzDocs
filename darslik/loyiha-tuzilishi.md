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
* `tests/`, ilova uchun testlarni o'z ichiga oluvchi jild.
* O'rnatish fayllari Python-ga loyihangizni qanday o'rnatishni aytadi.
* Versiyani boshqarish konfiguratsiyasi, masalan, git. Hajmidan qat'i nazar, barcha loyihalaringiz uchun versiya boshqaruvining ba'zi turlaridan foydalanishni odat qilishingiz kerak.
* Kelajakda qo'shishingiz mumkin bo'lgan boshqa loyiha fayllari.

Oxirida sizning loyihangiz sxemasi quyidagicha ko'rinadi:

```
/home/user/Projects/flask-tutorial
├── flaskr/
│   ├── __init__.py
│   ├── db.py
│   ├── schema.sql
│   ├── auth.py
│   ├── blog.py
│   ├── templates/
│   │   ├── base.html
│   │   ├── auth/
│   │   │   ├── login.html
│   │   │   └── register.html
│   │   └── blog/
│   │       ├── create.html
│   │       ├── index.html
│   │       └── update.html
│   └── static/
│       └── style.css
├── tests/
│   ├── conftest.py
│   ├── data.sql
│   ├── test_factory.py
│   ├── test_db.py
│   ├── test_auth.py
│   └── test_blog.py
├── .venv/
├── pyproject.toml
└── MANIFEST.in
```

Agar siz versiya boshqaruvidan foydalansangiz, loyihangizni ishga tushirishda yaratilgan quyidagi fayllar e'tiborga olinmasligi kerak. Foydalanadigan muharrirga asoslangan boshqa fayllar ham bo'lishi mumkin. Umuman olganda, siz yozmagan fayllarga e'tibor bermang. Masalan, git bilan:

{% code title=".gitignore" %}
```gitignore
.venv/

*.pyc
__pycache__/

instance/

.pytest_cache/
.coverage
htmlcov/

dist/
build/
*.egg-info/
```
{% endcode %}

