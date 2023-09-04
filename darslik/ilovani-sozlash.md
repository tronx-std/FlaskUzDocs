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
    # ilovani yaratish va sozlash
    app = Flask(__name__, instance_relative_config=True)
    app.config.from_mapping(
        SECRET_KEY='dev',
        DATABASE=os.path.join(app.instance_path, 'flaskr.sqlite'),
    )
    
    # ilova sozlamalarini yuklash
    if test_config is None:
        app.config.from_pyfile('config.py', silent=True)
    else:
        app.config.from_mapping(test_config)

    # ilova jildi mavjud ekanligini aniqlash
    try:
        os.makedirs(app.instance_path)
    except OSError:
        pass

    # salom yo'llaydigan oddiy route
    @app.route('/hello')
    def hello():
        return 'Hello, World!'

    return app
```
{% endcode %}

`create_app` - ilova zavod funksiyasi.

1. `app = Flask(__name__, instance_relative_config=True)` Flask nusxasini yaratadi.
   * \_\_name\_\_ joriy Python modulining nomi. Ba'zi yo'llarni o'rnatish uchun ilova qayerda joylashganligini bilishi kerak va \_\_name\_\_ buni aytishning qulay usulidir.
   * instance\_relative\_config=True ilovaga konfiguratsiya fayllari misol jildiga nisbatan ekanligini bildiradi. Namuna papkasi flaskr to'plamidan tashqarida joylashgan bo'lib, konfiguratsiya sirlari va ma'lumotlar bazasi fayli kabi versiya boshqaruviga berilmasligi kerak bo'lgan mahalliy ma'lumotlarni saqlashi mumkin.
2. app.config.from\_mapping() ilova foydalanadigan ba'zi standart konfiguratsiyalarni o'rnatadi:
   * SECRET\_KEY ma'lumotlarni xavfsiz saqlash uchun Flask va kengaytmalar tomonidan qo'llaniladi. Rivojlanish jarayonida qulay qiymatni ta'minlash uchun u "dev" ga o'rnatilgan, lekin uni joylashtirishda tasodifiy qiymat foydalanish kerak.
   * DATABASE - bu SQLite ma'lumotlar bazasi fayli saqlanadigan yo'l. Bu app.instance\_path ostida, ya'ni Flask papkasi uchun tanlagan yo'l. Keyingi bo'limda ma'lumotlar bazasi haqida ko'proq bilib olasiz.
3. app.config.from\_pyfile() agar mavjud bo'lsa, namuna jildidagi config.py faylidan olingan qiymatlar bilan standart konfiguratsiyani bekor qiladi. Misol uchun, deployda bu haqiqiy SECRET\_KEYni o'rnatish uchun ishlatilishi mumkin.
   * test\_config zavodga ham o'tkazilishi mumkin va standart konfiguratsiya o'rniga ishlatiladi. Shunday qilib, siz qo'llanmada keyinroq yozadigan testlar siz sozlagan har qanday rivojlanish qiymatlaridan mustaqil ravishda sozlanishi mumkin.
4. os.makedirs() app.instance\_path mavjudligini ta'minlaydi. Flask papkasini avtomatik ravishda yaratmaydi, lekin uni yaratish kerak, chunki sizning loyihangiz u erda SQLite ma'lumotlar bazasi faylini yaratadi.
5. @app.route() oddiy routeni yaratadi, shuning uchun qo'llanmaning qolgan qismiga kirishdan oldin dastur ishlayotganini ko'rishingiz mumkin. U URL /hello va javobni qaytaruvchi funksiya o'rtasida bog'lanish hosil qiladi, 'Hello, World!' Ushbu holatda.

## Ilovani ishga tushirish

Endi siz flask ilovasini `flask` buyrug'i yordamida ishga tushirishingiz mumkin. Ilovani ishga tushirish uchun terminalga quyidagi buyruqni yozing:

```bash
$ flask --app flaskr run --debug
```

Bu yerda --app argumenti bilan ilova qayerda ekanligini ko'rsatib o'tasiz va --debug orqali ilovnagizni debug mode-da ishga tushirasiz. Bu buyruqni yozganingizdan so'ng terminalda quyidagicha yozuv chiqadi:

```
* Serving Flask app "flaskr"
* Debug mode: on
* Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
* Restarting with stat
* Debugger is active!
* Debugger PIN: nnn-nnn-nnn
```

Bu sizning ilovangiz 127.0.0.1 hostida 5000-portda debug mode bilan ishlayotganini ko'rsatadi. Ilovangizni browserda ko'rish uchun http://127.0.0.1:5000/ ga boring.
