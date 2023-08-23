# Boshlash

Boshlashni xohlaysizmi? Ushbu sahifa Flaskga yaxshi kirish imkonini beradi. Loyihani o'rnatish va avval Flaskni o'rnatish uchun "O'rnatish" ga rioya qiling.

## Minimal dastur

Minimal Flask ilovasi quyidagicha ko'rinadi:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"
```

Xo'sh, bu kod nima qildi?

1. Avval biz Flask sinfini import qildik. Ushbu sinfning namunasi bizning WSGI ilovamiz bo'ladi.
2. Keyin biz ushbu sinfning namunasini yaratamiz. Birinchi argument dastur moduli yoki paketining nomidir. **\_\_name\_\_** - bu ko'p hollarda mos keladigan qulay yorliq. Bu Flask shablon va statik fayllar kabi resurslarni qayerdan qidirishni bilishi uchun kerak.
3. Keyin biz Flaskga qaysi URL bizning funktsiyamizni ishga tushirishi kerakligini aytish uchun route() dekoratoridan foydalanamiz.
4. Funktsiya foydalanuvchi brauzerida ko'rsatmoqchi bo'lgan xabarni qaytaradi. Standart kontent turi HTML, shuning uchun qatordagi HTML brauzer tomonidan ko'rsatiladi.

Uni hello.py yoki shunga o'xshash narsa sifatida saqlang. Ilovangizga flask.py nomi bilan saqlamang, chunki bu Flaskning o'ziga zid keladi.

Dasturni ishga tushirish uchun `flask` yoki `python -m flask` buyrug'idan foydalaning. Flaskga dasturingiz qayerda ekanligini `--app` argumenti bilan ko'rsating.

```bash
$ flask --app hello run
 * Serving Flask app 'hello'
 * Running on http://127.0.0.1:5000 (Press CTRL+C to quit)
```

Agar fayl `app.py` yoki `wsgi.py` nomi bilan saqlangan bo'lsa `--app` bilan dasturingiz qayerda ekanligini ko'rsatmasangiz ham bo'ladi.

Agar boshqa dastur 5000 port bilan ishlayotgan bo'lsa `OSError: [Errno 98]` yoki `OSError: [WinError 10013]` xatosini ko'rasiz.

Agar sizda debugger o'chirilgan bo'lsa yoki tarmog'ingizdagi foydalanuvchilarga ishonsangiz, buyruq satriga `--host=0.0.0.0` qo'shish orqali serverni hammaga ochiq qilishingiz mumkin:

```bash
$ flask run --host=0.0.0.0
```

Bu sizning operatsion tizimingizga barcha umumiy IP-larni tinglashni aytadi.

## Debug Mode

`flask run` buyrug'i development serverini ishga tushirishdan ko'ra ko'proq narsani qila oladi. Debug Mode'ni yoqish orqali, agar kod o'zgartirilsa, server avtomatik ravishda qayta yuklanadi va so'rov paytida xatolik yuzaga kelsa, brauzerda interaktiv debugger ko'rsatadi.

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

## <mark style="background-color:orange;">Ogohlantirish</mark>

<mark style="background-color:orange;">Debugger brauzerdan o'zboshimchalik bilan Python kodini bajarishga imkon beradi. U pin bilan himoyalangan, ammo baribir katta xavfsizlik xavfini anglatadi. Production muhitida devlopment serverini yoki debuggerni ishga tushirmang.</mark>

Debug Mode'ni yoqish uchun `--debug` argumentidan foydalaning

```bash
$ flask --app hello run --debug
 * Serving Flask app 'hello'
 * Debug mode: on
 * Running on http://127.0.0.1:5000 (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: nnn-nnn-nnn
```

HTML-ni (Flask-dagi standart javob turi) qaytarayotganda, in'ektsiya hujumlaridan himoya qilish uchun ko'rsatilgan foydalanuvchi tomonidan taqdim etilgan qiymatlardan qochish kerak. Keyinchalik taqdim etilgan Jinja bilan yaratilgan HTML shablonlari buni avtomatik ravishda bajaradi.

Bu yerda ko'rsatilgan escape(), qo'lda ishlatilishi mumkin. Ko'pgina misollarda qisqalik uchun olib tashlangan, ammo ishonchsiz ma'lumotlardan qanday foydalanayotganingizdan doimo xabardor bo'lishingiz kerak.

```python
from markupsafe import escape

@app.route("/<name>")
def hello(name):
    return f"Hello, {escape(name)}!"
```

Agar foydalanuvchi `<script>alert("bad")<script>` nomini yuborishga muvaffaq bo'lsa, escaping uni foydalanuvchi brauzerida skriptni ishga tushirish o'rniga matn sifatida ko'rsatishga olib keladi.

Route'agi `<name>` URL manzilidan qiymatni oladi va uni funktsiyaga o'tkazadi. Ushbu o'zgaruvchan qoidalar quyida tushuntiriladi.

## Routing

Zamonaviy veb-ilovalar foydalanuvchilarga yordam berish uchun mazmunli URL-lardan foydalanadi. Agar sahifada eslab qolishlari va sahifaga bevosita tashrif buyurish uchun foydalanishlari mumkin bo'lgan mazmunli URL ishlatilsa, foydalanuvchilar sahifani yoqtirishlari va qaytib kelishlari ehtimoli ko'proq.

Funktsiyani URL manziliga ulash uchun route() dekoratoridan foydalaning.

```python
@app.route('/')
def index():
    return 'Index Page'

@app.route('/hello')
def hello():
    return 'Hello, World'
```

Siz ko'proq narsani qila olasiz! Siz URL qismlarini dinamik qilishingiz va funksiyaga bir nechta qoidalarni biriktirishingiz mumkin.

## O'zgaruvchan qoidalar

Bo'limlarni `<o'zgaruvchi_nomi>` bilan belgilash orqali URL manziliga o'zgaruvchilar bo'limlarini qo'shishingiz mumkin. Keyin funktsiyangiz kalit so'z argumenti sifatida `<variable_name>` ni oladi. _Ixtiyoriy_, argument turini belgilash uchun konvertordan foydalanishingiz mumkin, masalan, `<konvertor: o'zgaruvchan_nomi>`.

```python
from markupsafe import escape

@app.route('/user/<username>')
def show_user_profile(username):
    # show the user profile for that user
    return f'User {escape(username)}'

@app.route('/post/<int:post_id>')
def show_post(post_id):
    # show the post with the given id, the id is an integer
    return f'Post {post_id}'

@app.route('/path/<path:subpath>')
def show_subpath(subpath):
    # show the subpath after /path/
    return f'Subpath {escape(subpath)}'
```

Converter types:

| `string` | (standart) har qanday matnni slash belgisisiz qabul qiladi |
| -------- | ---------------------------------------------------------- |
| `int`    | musbat sonlarni qabul qiladi                               |
| `float`  | musbat haqiqiy sonlarni qabul qiladi                       |
| `path`   | `string` kabi, lekin slashlarni ham qabul qiladi           |
| `uuid`   | UUID satrlarini qabul qiladi                               |

## Noyob URL manzillari / Qayta yo'naltirish harakati

Quyidagi ikkita qoida keyingi slashdan foydalanishda farqlanadi.

```python
@app.route('/projects/')
def projects():
    return 'Loyihalar sahifasi'

@app.route('/about')
def about():
    return 'Ma\'lumot sahifasi'
```

`projects` so'nggi nuqtasi uchun kanonik URLda slash bor. Bu fayl tizimidagi papkaga o'xshaydi. Agar siz URL manziliga slashsiz (`/projects`) kirsangiz, Flask sizni keyingi slash (`/projects/`) bilan kanonik URL manziliga yo‘naltiradi.

`about` so'nggi nuqtasi uchun kanonik URLda slash yo'q. Bu faylning yo'l nomiga o'xshaydi. URL manziliga slash (`/about/`) bilan kirish 404 “Not Found” xatosini keltirib chiqaradi. Bu URL manzillarini ushbu manbalar uchun noyob saqlashga yordam beradi, bu esa qidiruv tizimlariga bir sahifani ikki marta indekslashni oldini olishga yordam beradi.

## URL qurish

Muayyan funktsiyaning URL manzilini yaratish uchun url\_for() funksiyasidan foydalaning. U birinchi argument sifatida funktsiya nomini va har bir URL qoidasining o'zgaruvchan qismiga mos keladigan kalit so'z argumentlarining istalgan sonini qabul qiladi. Noma'lum o'zgaruvchan qismlar URL manziliga so'rov parametrlari sifatida qo'shiladi.

Nima uchun URL manzillarini shablonlaringizga hard-coding[^1] o‘rniga url\_for() funksiyasidan foydalanib yaratmoqchisiz?

1. `url_for()` ko'pincha URL-manzillarni hard-coding'dan ko'ra tavsifliroq.
2. hard-coded URL-manzillarni qo'lda o'zgartirishni eslab qolish o'rniga, URL-manzillaringizni bir marta o'zgartirishingiz mumkin.
3. URL yaratish maxsus belgilardan qochishni shaffof tarzda boshqaradi.
4. Yaratilgan URL har doim mutlaq bo'lib, brauzerlarda nisbiy URLlarning kutilmagan xatti-harakatlaridan qochadi.
5. Agar ilovangiz URL ildizidan tashqarida joylashgan bo'lsa, masalan, / o'rniga /myapplication-da, url\_for() buni siz uchun to'g'ri bajaradi.

Misol uchun, bu  yerda url\_for() ni sinab ko'rish uchun test\_request\_context() usulidan foydalanamiz. test\_request\_context() Flaskga o'zini Python qobig'idan foydalanganda ham xuddi so'rovni bajarayotgandek tutishni aytadi.

```python
from flask import url_for

@app.route('/')
def index():
    return 'index'

@app.route('/login')
def login():
    return 'login'

@app.route('/user/<username>')
def profile(username):
    return f'{username}\'s profile'

with app.test_request_context():
    print(url_for('index'))
    print(url_for('login'))
    print(url_for('login', next='/'))
    print(url_for('profile', username='John Doe'))
```

```
/
/login
/login?next=/
/user/John%20Doe
```

## HTTP Metodlar

Veb-ilovalar URL manzillariga kirishda turli HTTP metodlardan foydalanadi. Flask bilan ishlashda HTTP metodlari bilan tanishishingiz kerak. Odatda `route` faqat GET so'rovlariga javob beradi. Turli HTTP metodlarini boshqarish uchun `route()` dekoratorining `methods` argumentidan foydalanishingiz mumkin.

```python
from flask import request

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        return do_the_login()
    else:
        return show_the_login_form()
```

Yuqoridagi misol `route`ning barcha usullarini bitta funktsiya doirasida saqlaydi, bu har bir qism ba'zi umumiy ma'lumotlardan foydalansa foydali bo'lishi mumkin.

Bundan tashqari, turli metodlar uchun `view`larni turli funktsiyalarga ajratishingiz mumkin. Flaskda har bir umumiy HTTP metodi uchun `route` dekoratori o'rnida ishlatish uchun `get()`, `post()` kabi dekoratorlar bor.&#x20;

```python
@app.get('/login')
def login_get():
    return show_the_login_form()

@app.post('/login')
def login_post():
    return do_the_login()
```

Agar `GET` mavjud bo'lsa, Flask avtomatik ravishda `HEAD` metodini qo'llab-quvvatlaydi va [HTTP RFC](https://www.ietf.org/rfc/rfc2068.txt) ga muvofiq `HEAD` so'rovlarini bajaradi. Xuddi shunday, OPTIONS siz uchun avtomatik ravishda amalga oshiriladi.

## Statik fayllar

Dinamik veb-ilovalar uchun statik fayllar ham kerak bo'ladi. Bu odatda CSS va JavaScript fayllari qayerdan kelishini ko'rsatadi. Ideal holda sizning veb-serveringiz ular uchun avtomatik sozlangan, ammo Flask ishlab chiqish jarayonida buni ham o'zgartira oladi. Paketingizda yoki modulingiz yonida `static` nomli jild yarating va u ilovada /static da ko'rinadi.&#x20;

Statik fayllar uchun URL manzillarini yaratish uchun maxsus `"static"` nomidan foydalaning:

```python
url_for('static', filename='style.css')
```

Fayl fayl tizimida `static/style.css` sifatida saqlanishi kerak.

## Shablonlar(HTML sahifa) ko'rsatish

Python ichida HTML yaratish qiziq emas va aslida juda mashaqqatli, chunki dasturni xavfsiz saqlash uchun "HTML escaping" ni o'zingiz qilishingiz kerak. Shu sababli Flask siz uchun Jinja2 shablon mexanizmini avtomatik ravishda sozlaydi.

Shablonlar har qanday turdagi matn faylini yaratish uchun ishlatilishi mumkin. Veb-ilovalar uchun siz birinchi navbatda HTML sahifalarini yaratasiz, lekin siz "markdown", elektron pochta xabarlari uchun oddiy matn va boshqa narsalarni ham yaratishingiz mumkin.

HTML,CSS,JavaScript va boshqa web API'larni o'rganish uchun [MDN Web Docs](https://developer.mozilla.org/) dan foydalaning.

Shablonlarni yaratish uchun `render_tamplate()` funktsiyasidan foydalanishingiz mumkin. Siz qilishingiz kerak bo'lgan narsa shablon nomini va shablon mexanizmiga kalit so'z argumentlari sifatida o'tkazmoqchi bo'lgan o'zgaruvchilarni taqdim etishdir. Shablonni qanday yaratishga oddiy misol:

```python
from flask import render_template

@app.route('/hello/')
@app.route('/hello/<name>')
def hello(name=None):
    return render_template('hello.html', name=name)
```

Flask `templates` papkasida shablonlarni qidiradi. Shunday qilib, agar ilovangiz modul bo'lsa, bu jild o'sha modul yonida, agar u paket bo'lsa, u paketingiz ichida bo'ladi:

**1-holat**: modul:

```
/application.py
/templates
    /hello.html
```

**2-holat**: paket:

```
/application
    /__init__.py
    /templates
        /hello.html
```

Shablonlar uchun siz Jinja2 shablonlarining to'liq quvvatidan foydalanishingiz mumkin. Qo'shimcha ma'lumot olish uchun rasmiy [Jinja2 Templates Documentation](https://jinja.palletsprojects.com/templates/)ga o'ting.

Namuna Shablon:

```django
<!doctype html>
<title>Hello from Flask</title>
{% raw %}
{% if name %}
  <h1>Hello {{ name }}!</h1>
{% else %}
  <h1>Hello, World!</h1>
{% endif %}
{% endraw %}
```

Shablonlar ichida siz `config`, `request`, `session` va `g` ob'yektlari hamda `url_for()` va `get_flashed_messages()` funksiyalariga kirishingiz mumkin.

## Request ob'yekti

Bu yerda ba'zi eng keng tarqalgan operatsiyalarning keng ko'rinishi mavjud. Avvalo uni flask modulidan import qilishingiz kerak:

```python
from flask import request
```

Joriy so'rov metodi `method` atributi yordamida mavjud. Forma ma'lumotlariga kirish uchun (POST yoki PUT so'rovida uzatiladigan ma'lumotlar) siz `form` atributidan foydalanishingiz mumkin. Yuqorida aytib o'tilgan ikkita atributning to'liq misoli:

```python
@app.route('/login', methods=['POST', 'GET'])
def login():
    error = None
    if request.method == 'POST':
        if valid_login(request.form['username'],
                       request.form['password']):
            return log_the_user_in(request.form['username'])
        else:
            error = 'Invalid username/password'
    # the code below is executed if the request method
    # was GET or the credentials were invalid
    return render_template('login.html', error=error)
```

Form atributida kalit mavjud bo'lmasa nima bo'ladi? Bunday holda, maxsus KeyError paydo bo'ladi. Siz uni standart KeyError kabi ushlab qolishingiz mumkin, lekin buni qilmasangiz, uning o'rniga HTTP 400 Bad Request sahifasi ko'rsatiladi. Shunday qilib, ko'p holatlarda siz bu muammoni hal qilishingiz shart emas.

URL manzilida (?key=value) taqdim etilgan parametrlarga kirish uchun args atributidan foydalanishingiz mumkin:

```python
searchword = request.args.get('key', '')
```

Biz URL parametrlariga GET yordamida yoki KeyError ni ushlash orqali kirishni tavsiya qilamiz, chunki foydalanuvchilar URL manzilini oʻzgartirishi va ularga 400 Bad Requet sahifasini taqdim etishi foydalanuvchilar uchun qulay emas.

## Fayl yuklash

Siz flask bilan yuklangan fayllarni oson ushlab qolishingiz mumkin. Shunchaki HTML sahifangizdagi formada `enctype="multipart/form-data"`ni qo'shganingizga ishonch hosil qiling. Aks holda brauzer sizning fayllaringizni umuman uzatmaydi.

Yuklangan fayllar fayl tizimidagi vaqtinchalik joyda saqlanadi. Siz ushbu fayllarga `request` ob'ektidagi `files` atributi bilan kirishingiz mumkin. Har bir yuklangan fayl ushbu dict'da saqlanadi.  U xuddi standart Python fayl ob'ekti kabi ishlaydi, lekin u shuningdek, ushbu faylni serverning fayl tizimida saqlash imkonini beruvchi save() funktsiyasiga ega. Bu qanday ishlashini ko'rsatadigan oddiy misol:

```python
from flask import request

@app.route('/upload', methods=['GET', 'POST'])
def upload_file():
    if request.method == 'POST':
        f = request.files['the_file']
        f.save('/var/www/uploads/uploaded_file.txt')
    ...
```

Agar fayl ilovangizga yuklanishidan oldin mijozda qanday nomlanganini bilmoqchi bo'lsangiz, `filename` atributiga kirishingiz mumkin. Ammo shuni yodda tutingki, bu qiymat soxtalashtirilishi mumkin, shuning uchun hech qachon bu qiymatga ishonmang. Agar siz faylni serverda saqlash uchun mijozning fayl nomidan foydalanmoqchi boʻlsangiz, Werkzeug sizga taqdim etgan security\_filename() funksiyasidan foydalaning:

```python
from werkzeug.utils import secure_filename

@app.route('/upload', methods=['GET', 'POST'])
def upload_file():
    if request.method == 'POST':
        file = request.files['the_file']
        file.save(f"/var/www/uploads/{secure_filename(file.filename)}")
    ...
```

[^1]: dasturda (ma'lumotlar yoki parametrlarni) dasturni o'zgartirmasdan o'zgartirib bo'lmaydigan tarzda tuzatish.
