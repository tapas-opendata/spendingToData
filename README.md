# Як оприлюднити звітність та договори зі spending.gov.ua на data.gov.ua

## Вступ
На бухгалтерів покладена велика кількість обов’язків пов’язаних з оприлюднення відкритих даних. Робота з [Prozorro](https://prozorro.gov.ua/), [Єдиним веб-порталом використання публічних фінансів (spending.gov.ua)](https://spending.gov.ua/), [Єдиним державним порталом відкритих даних (data.gov.ua)](https://data.gov.ua/) забирає багато часу, а розміщена інформація частково дублюється. [Постанова КМУ №835 (зі змінами)](http://zakon0.rada.gov.ua/laws/show/835-2015-%D0%BF) вимагає від органів місцевого самоврядування оприлюднювати набір даних **"Перелік укладених договорів, укладені договори, інші правочини, додатки, додаткові угоди та інші матеріали до них"**. Також органи місцевого самоврядування можуть зобов'язати комунальні підприємства оприлюднювати **фінансову звітність у формі відкритих даних**. Дані з цих наборів вже частково оприлюднені на spending.gov.ua. Тому, дублювати їх на data.gov.ua або місцевих порталах відкритих даних недоцільно. Електронні таблиці (XLS, XLSX, ODS), що зазвичай використовуються в таких випадках, мають низку цінність через заголовки та об’єднані комірки. Значно ефективніше оприлюднити дані зі spending.gov.ua на data.gov.ua через API (детальніше що це, і як це зробити розглянемо нижче). Решту даних, які не оприлюднюються на spending.gov.ua портібно додати у звичних електронних таблицях або CSV (якщо дані гарно структуровані). 
**Яких результатів можна досягти впровадивши ці рекомендації.**
1. **Зменшення робочого навантаження на бухгалтерів та відповідальних осіб.** Дані зі spending.gov.ua потрібно оприлюднити на data.gov.ua всього 1 раз. Надалі вони оновлюватимуться автоматично. Після цього регулярно оновлювати доведеться лише звітність та договори які не заносяться в spending.gov.ua.
2. **Якісні дані.** spending.gov.ua віддаватиме користувачам вже готові до аналізу та використання у програмному забезпеченні дані у машиночитаному форматі (JSON).
3. **Надійність.** Іноді бухгалтери неохоче оприлюднюють документи “без печаток”, бо користувачі можуть легко змінити інформацію в них. Дані, що оприлюднюються на spending.gov.ua, підписані електронним цифровим підписом.

## Що таке API spending.gov.ua?
Сервіси в мережі Інтернет мають бази даних, де зберігають інформацію користувачів. Прикладами баз spending.gov.ua є бази даних звітів, транзакцій, договорів та інше. Для того, щоб дати доступ до цих баз через мережу Інтернет, розробники створюють спеціальний функціонал, який носить назву API (з англ. - інтерфейс прикладного програмування). 

Всім відомо, що кожна сторінка в мережі Інтернет має власну унікальну адресу, наприклад, [`https://dniprorada.gov.ua/`](https://dniprorada.gov.ua/) або [`https://www.lutskrada.gov.ua/`](https://www.lutskrada.gov.ua/), так само і дані кожного розпорядника на spending.gov.ua мають свою адресу. Наприклад, перелік договорів Дніпровської міської ради (номер в ЄДР - 26510514) можна отрмати за адресою

[```http://api.spending.gov.ua/api/v2/disposers/contracts?disposerId=26510514```](http://api.spending.gov.ua/api/v2/disposers/contracts?disposerId=26510514)

Виконавчого комітету Луцької міської ради (номер в ЄДР - 04051327) - 

[```http://api.spending.gov.ua/api/v2/disposers/contracts?disposerId=04051327```](http://api.spending.gov.ua/api/v2/disposers/contracts?disposerId=04051327)

Такий спосіб отримання даних можна можна умовно назвати API. **Якщо порівняти посилання, то відрізняються вони лише останніми цифрами - номером в ЄДР.** API spending.gov.ua повертає дані у форматі JSON. Він не зрозумілий для сприйняття людини але якнайкраще підходить для машинної обробки. Тому, якщо для вас є незрозумілою велика кількість символів на моніторі - це природно. Переглянути ієрархічну структуру JSON-файлів можна за допомогою додатку [JSONView](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc/) до веб-переглядача Google Chrome.

```
{
  "size": 1000,
  "count": 2311,
  "moreThenCount": false,
  "documents": [
    {
      "id": 1450996651,
      "edrpou": "04051327",
      "documentNumber": "390",
      "documentDate": "2018-10-19",
      "signDate": "2018-11-02",
      "signature": {
        "caAddress": "ca.informjust.ua"
      },
      "amount": 2325.07,
      "currency": "UAH",
      "currencyAmountUAH": 0,
      "contractors": [
        {
          "identifier": "00182857",
          "contractorType": 2,
          "name": "ВОЛИНСЬКЕ ОБЛАСНЕ СПЕЦІАЛІЗОВАНЕ РЕМОНТНО-БУДІВЕЛЬНЕ ПІДПРИЄМСТВО ПРОТИПОЖЕЖНИХ РОБІТ",
          "firstName": "Г.",
          "lastName": "ЗАГОРОДНІЙ",
          "middleName": "І.",
          "address": {...}
        }
      ],
      "fromDate": "2018-10-19",
      "toDate": "2018-12-31",
      "subject": "ПРОВЕДЕННЯ РОБІТ З ТЕХНІЧНОГО ОБСЛУГОВУВАННЯ ВОГНЕГАСНИКІВ",
      "noTerm": false,
      "pdvInclude": true,
      "pdvAmount": 387.51,
      "tender": false,
      "reason": "ч.1 ст. 2 закону України Про державні закупівлі",
      "specifications": [],
      "isCpvVat": false,
      "procurementItems": []
    },
    {...},
    {...},
    ...
    {...}
  ]
}

```


Тепер можна спробувати навмисне ввести помилковий номер в ЄДР (00000000): 

[```http://api.spending.gov.ua/api/v2/disposers/contracts?disposerId=00000000```](http://api.spending.gov.ua/api/v2/disposers/contracts?disposerId=00000000)

Після завантаження портал поверне коротке повідомлення:

```{"size":0,"count":0,"moreThenCount":false,"documents":[]}```

Воно означає, що договорів за таким номером не знайдено. Якщо ви введете справжній номер у ЄДР і побачите таке повідомлення, це може означати помилку в номері або про те, що розпорядник не завантажив звітність.

## Як оприлюднити набір Перелік укладених договорів за допомогою spending.gov.ua

Набір даних "Перелік укладених договорів, укладені договори, інші правочини, додатки, додаткові угоди та інші матеріали до них" може включати наступні ресурси:
1. **contracts** - перелік договорів оприлюднених на Єдиному веб-порталі використання публічних фінансів (spending.gov.ua);
2. **addendums** - перелік додаткових угод оприлюднених на Єдиному веб-порталі використання публічних фінансів (spending.gov.ua);
3. **acts** - перелік актів до договорів оприлюднених на Єдиному веб-порталі використання публічних фінансів (spending.gov.ua)
4. **peny** - перелік штрафів до договорів оприлюднених на Єдиному веб-порталі використання публічних фінансів (spending.gov.ua)
5. **otherContracts** - перелік інших договорів, що укладені розпорядником та не підпадають під Закон України “Про відкритість використання публічних коштів”.

Для того щоб отримати дані зі spending.gov.ua треба використати наступні шаблони посилань:
- для договорів - `http://api.spending.gov.ua/api/v2/disposers/contracts?disposerId=ЄДРПОУ`
- для додаткових угод - `http://api.spending.gov.ua/api/v2/disposers/addendums?disposerId=ЄДРПОУ`
- для актів - `http://api.spending.gov.ua/api/v2/disposers/acts?disposerId=ЄДРПОУ`
- для штрафів - `http://api.spending.gov.ua/api/v2/disposers/peny?disposerId=ЄДРПОУ`

Останню частину ЄДРПОУ в усіх шаблонах треба замінити на номер розпорядника в ЄДР. Наприклад, якщо номер розпорядника в ЄДР - 04051327, то посилання виглядатимуть так:
- для договорів - [`http://api.spending.gov.ua/api/v2/disposers/contracts?disposerId=04051327`](http://api.spending.gov.ua/api/v2/disposers/contracts?disposerId=04051327)
- для додаткових угод - [`http://api.spending.gov.ua/api/v2/disposers/addendums?disposerId=04051327`](http://api.spending.gov.ua/api/v2/disposers/addendums?disposerId=04051327)
- для актів - [`http://api.spending.gov.ua/api/v2/disposers/acts?disposerId=04051327`](http://api.spending.gov.ua/api/v2/disposers/acts?disposerId=04051327)
- для штрафів - [`http://api.spending.gov.ua/api/v2/disposers/peny?disposerId=04051327`](http://api.spending.gov.ua/api/v2/disposers/peny?disposerId=04051327)

Ці посилання і треба оприлюднити в наборі даних. Тепер перейдемо до роботи з порталом data.gov.ua. Почнемо  зі створення набору та заповнення його паспорту.

**Таблиця 1 - Приклад паспорту набору даних на data.gov.ua**

| Назва поля | Приклад заповнення |
| ------ | ------ |
| Назва набору | Перелік укладених договорів, укладені договори, інші правочини, додатки, додаткові угоди та інші матеріали до них |
| Відомості про мову інформації, яка міститься у наборі | українська |
| Частота оновлення | один раз на квартал |
| Опис | Набір містить перелік укладених договорів, укладені договори, інші правочини, додатки, додаткові угоди та інші матеріали до них Виконавчого комітету Луцької міської ради. Дані з Єдиного веб-порталу використання публічних фінансів оприлюднені через API. Решта даних, яка не підпадає під Закон України “Про відкритість використання публічних коштів”, оприлюднена файлами. Довідка, щодо використання API spending.gov.ua доступна за посиланням - https://confluence.spending.gov.ua/. |
| Підстава та призначення збору інформації | Виконання повноважень відповідно до Закону України “Про бухгалтерський облік та фінансову звітність в Україні”, Закону України “Про відкритість використання публічних коштів”, Закону України “Про публічні закупівлі” та підзаконних актів. |
| Ключові слова | договір, угода, акт, штраф, пеня, додаткова угода, додаток, товари, послуги |
| Відповідальна особа | Симоненко Олена Петрівна |
| Адреса електронної пошти відповідальної особи | o.symonenko@example.gov.ua |

На наступному кроці додаємо ресурси у вкладці завантажити дані.

**Таблиця 2 - Приклад додавання ресурсу contracts**

| Назва поля | Приклад заповнення |
| ------ | ------ |
| Посилання | http://api.spending.gov.ua/api/v2/disposers/contracts?disposerId=04051327 |
| Назва ресурсу | contracts |
| Опис |  Перелік договорів оприлюднених на Єдиному веб-порталі використання публічних фінансів (spending.gov.ua). Довідка, щодо використання API spending.gov.ua доступна за посиланням - https://confluence.spending.gov.ua/. |
| Формат | API |

**Таблиця 3 - Приклад додавання ресурсу addendums**

| Назва поля | Приклад заповнення |
| ------ | ------ |
| Посилання | http://api.spending.gov.ua/api/v2/disposers/addendums?disposerId=04051327 |
| Назва ресурсу | addendums |
| Опис | Перелік додаткових угод оприлюднених на Єдиному веб-порталі використання публічних фінансів (spending.gov.ua). Довідка, щодо використання API spending.gov.ua доступна за посиланням - https://confluence.spending.gov.ua/. |
| Формат | API |

**Таблиця 4 - Приклад додавання ресурсу acts**

| Назва поля | Приклад заповнення |
| ------ | ------ |
| Посилання | http://api.spending.gov.ua/api/v2/disposers/acts?disposerId=04051327 |
| Назва ресурсу | acts |
| Опис | Перелік актів до договорів оприлюднених на Єдиному веб-порталі використання публічних фінансів (spending.gov.ua). Довідка, щодо використання API spending.gov.ua доступна за посиланням - https://confluence.spending.gov.ua/. |
| Формат | API |

**Таблиця 5 - Приклад додавання ресурсу peny**

| Назва поля | Приклад заповнення |
| ------ | ------ |
| Посилання | http://api.spending.gov.ua/api/v2/disposers/peny?disposerId=04051327 |
| Назва ресурсу | peny |
| Опис | Перелік штрафів до договорів оприлюднених на Єдиному веб-порталі використання публічних фінансів (spending.gov.ua). Довідка, щодо використання API spending.gov.ua доступна за посиланням - https://confluence.spending.gov.ua/. |
| Формат | API |

На spending.gov.ua оприлюднюється лише документи, що підпадають під виконання [Закону України "Про відкритість використання публічних коштів"](http://zakon3.rada.gov.ua/laws/show/183-19). Тим часом, розпорядники можуть мати й інші типи договорів, наприклад, про оренду, приватизацію комунального майна, розміщення тимчасових споруд, рекламних засобів, виконання інвестиційних проектів тощо. Тому, для таких угод необхідно створити окремий реєстр. Для таблиць реєстру можна використати [шаблони для імпорту договорів, додаткових угод, актів, штрафів у spending.gov.ua](https://confluence.spending.gov.ua/pages/viewpage.action?pageId=10715871).

**Таблиця 6 - Приклад додавання ресурсу otherContracts**

| Назва поля | Приклад заповнення |
| ------ | ------ |
| Файл | otherContracts-2018-kv-01.xlsx |
| Назва ресурсу | otherContracts-2018-kv-01 |
| Опис | Перелік інших договорів, що укладені розпорядником за 1 квартал 2018 року та не підпадають під Закон України “Про відкритість використання публічних коштів”. |
| Формат | XLSX |



| Назва поля | Приклад заповнення |
| ------ | ------ |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |





