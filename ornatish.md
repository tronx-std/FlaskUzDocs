# O'rnatish

## Python Versiyasi

Biz pythonning eng so'nggi versiyasidan foydalanishni maslahat beramiz. Flask pythonning 3.8 va yangiroq versiyalarini qo'llab quvvatlaydi.

## Kerakli kutubxonalar

Bu kutubxonalar flask o'rnatayotganingizda avtomatik o'rnatiladi.

* [Werkzeug](https://palletsprojects.com/p/werkzeug/) dastur va server o'rtasidagi standart Python interfeysi WSGI-ni ishga tushuradi.
* [Jinja](https://palletsprojects.com/p/jinja/) - bu ilovangiz xizmat ko'rsatadigan sahifalarni ko'rsatadigan shablon tili.
* [Markupsafe](https://palletsprojects.com/p/markupsafe/) Jinja bilan birga keladi. U inyeksiya hujumlaridan qochish uchun shablonlarni ko'rsatishda ishonchsiz kiritishdan qochadi.
* [ItsDangerous](https://palletsprojects.com/p/itsdangerous/) ma'lumotlarning yaxlitligini ta'minlash uchun xavfsiz tarzda imzolaydi. Bu Flask seans cookie faylini himoya qilish uchun ishlatiladi.
* [Click](https://palletsprojects.com/p/click/) - bu buyruq qatori ilovalarini yozish uchun ramka. U flask buyrug'ini beradi va maxsus boshqaruv buyruqlarini qo'shish imkonini beradi.
* [blinker](https://blinker.readthedocs.io/) - Signallarni qo'llab-quvvatlaydi.

## Ixtioriy kutubxonalar

Bu kutubxonalar avtomatik o'rnatilmaydi. Agar o'rnatilgan bo'lsa flask buni aniqlaydi va foydalanadi.

* [python-dotenv](https://github.com/theskumar/python-dotenv#readme) flask buyruqlarini bajarayotganda dotenv dan muhit o'zgaruvchilarini qo'llab-quvvatlashga imkon beradi.
* [Watchdog](https://pythonhosted.org/watchdog/) ishlab chiqish serveri uchun tezroq va samaraliroq qayta yuklashni ta'minlaydi.

## Virtual muhit

Virtual muhitdan loyihangizdagi kutubxonalarni kompyuteringizda va serverda birdek boshqarish uchun ishlating.&#x20;

Virtual muhit qanday muammoni bartaraf etadi? Sizda ko'plab loyihalar bor va har birida sizga python kutubxonalaring va hattoki pythonning o'zining ham turli xil versiyalari kerak bo'ladi. Bir loyihadagi kutubxonaning yangiroq versiyasi boshqa bir loyihani ishdan chiqarishi mumkin.

Virtual muhitlar har bir loyiha uchun python kutubxonalarining mustaqil guruxi. Bir loyihada o'rnatilgan kutubxona boshqa loyihaga ta'sir qilmaydi.

Pythonni o'rnatganingizda virtual muhitlar yaratish uchun venv moduli ham avtomatik tarzda o'rnatiladi.

## Virtual muhit o'rnatish

Loyiha jildini va uning ichida .venv jildini yaratamiz:

{% code title="macOS/Linux" fullWidth="false" %}
```sh
$ mkdir myproject
$ cd myproject
$ python3 -m venv .venv
```
{% endcode %}

{% code title="Windows" %}
```sh
> mkdir myproject
> cd myproject
> py -3 -m venv .venv
```
{% endcode %}

## Virtual muhitni ishga tushirish

Loyihangiz ustida ish boshlashdan oldin virtual muhitni ishga tushiring:

{% code title="macOS/Linux" %}
```bash
$ . .venv/bin/activate
```
{% endcode %}

{% code title="Windows" %}
```sh
> .venv\Scripts\activate
```
{% endcode %}

## Flaskni o'rnatish

Ishga tushirilgan muhitda quyidagi buyruqni yozing:

```sh
$ pip install Flask
```

Flask o'rnatildi.
