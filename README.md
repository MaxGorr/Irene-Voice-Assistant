# Голосовой ассистент Ирина

Ирина - русский голосовой ассистент для работы оффлайн. Требует Python 3.5+ (зависимость может быть меньше, но в любом случае Python 3)

Поддерживает плагины (скиллы)

[Статья на Хабре](https://habr.com/ru/post/595855/) | [Вторая статья на Хабре](https://habr.com/ru/post/660715/) | [Группа в Телеграм](https://t.me/irene_va)

### Самая быстрая установка под Windows

1. Зайдите в релизы: https://github.com/janvarev/Irene-Voice-Assistant/releases 
2. Скачайте релиз и следуйте инструкции. Python и GIT идут в релизе, ничего ставить не нужно.

После установки будут доступны оффлайн-команды (т.к. это дефолтовая конфигурация). 
Пример: "ирина привет", "ирина подбрось монетку", "ирина подбрось кубик", "ирина игра больше меньше", "ирина таймер три минуты"

### Установка / быстрый старт

Вам потребуется установленный Python.

1. Для быстрой установки всех требуемых зависимостей можно воспользоваться командой:
```pip install -r requirements.txt```
(Для Linux и macOS - предварительно установите пакеты для [audioplayer](https://pypi.org/project/audioplayer/))

2. Для запуска запустите файл **runva_vosk.py** из корневой папки.
По умолчанию он запустит оффлайн-распознаватель vosk для распознавания речи с микрофона, 
и pyttsx движок для озвучивания ассистента 
[Подробнее о pyttsx здесь](https://github.com/nateshmbhat/pyttsx3).

3. После запуска проверить можно простой командой - скажите "Ирина, привет!" в микрофон

**Папка с настройками options появится после первого запуска Ирины, в ней можно поправить настройки.**

Более пошаговая инфа про установку на Win (в особенности Win 7): [docs/INSTALL_WIN.md](/docs/INSTALL_WIN.md)

Решение некоторых проблем при установке под Linux: [docs/INSTALL_LINUX.md](/docs/INSTALL_LINUX.md)

Решение некоторых проблем при установке под Mac: [docs/INSTALL_MAC.md](/docs/INSTALL_MAC.md)

Принципы отладки при проблемах при установке: [docs/INSTALL_DEBUG.md](/docs/INSTALL_DEBUG.md)

Баги можно писать в ISSUES, обсуждать - [в Телеграм](https://t.me/+gagUw1bCcYFkMGEy)

### Установка через Докер

Если хотите ВСЁ запустить через Докер: [docs/INSTALL_DOCKER.md](/docs/INSTALL_DOCKER.md)

Если хотите только сложные ключевые компоненты запустить через Докер: [docs/INSTALL_DOCKER_COMP.md](/docs/INSTALL_DOCKER_COMP.md)

### Общая логика

Запуск всех команд начинается с имени ассистента (настраивается в options/core.json, по умолчанию - Ирина). 
Так сделано, чтобы исключить неверные срабатывания при постоянном прослушивании микрофона.
Далее будут описываться команды без префикса "Ирина".

В движок встроена поддержка локального управления через веб-интерфейс плейером MPC-HC, так что при прочих равных рекомендуется использовать его. 
Его можно настроить в options/core.json

## Плагины

Поддержка плагинов сделана на собственном движке [Jaa.py](https://github.com/janvarev/jaapy) - минималистичный однофайловый движок поддержки плагинов и их настроек.

Плагины располагаются в папке plugins и должны начинаться с префикса "plugins_".

Настройки плагинов, если таковые есть, располагаются в папке "options" (создается после первого запуска).

### Готовые плагины/скиллы (уже в папке plugins)

Для каждого плагина написано, требуется ли онлайн. 
Для отключения удалите из папки

Полная информация: [docs/PLUGINS.md](/docs/PLUGINS.md)

### Сторонние плагины

Если вы хотите узнать:
  * какие еще есть плагины от других разработчиков
  * запостить ссылку на свой сделанный плагин
  
Посетите: [https://github.com/janvarev/Irene-Voice-Assistant/issues/1](https://github.com/janvarev/Irene-Voice-Assistant/issues/1)

### Интеграция с Home Assistant

Есть хороший сторонний плагин, позволяющий запускать сценарии Home Assistant через Ирину:
https://github.com/timhok/IreneVA-hassio-script-trigger-plugin

### Настройки ядра (core.json)

Настройки конкретных плагинов лучше смотреть в плагинах

```python
{
    "isOnline": true, # при установке в false будет выдавать заглушку на команды плагинов, требующих онлайн. Рекомендуется, если нужен только оффлайн.
    "linguaFrancaLang": "ru", # язык для конвертации чисел в lingua-franca. Смените, если будете работать с другим языком
    "logPolicy": "cmd", # all|cmd|none . Когда распознается речь с микрофона - выводить в консоль всегда | только, если является командой | никогда
    "mpcHcPath": "C:\\Program Files (x86)\\K-Lite Codec Pack\\MPC-HC64\\mpc-hc64_nvo.exe", # путь до MPC HC, если используете
    "mpcIsUse": true, # используется ли MPC HC?
    "mpcIsUseHttpRemote": true, # MPC HC - включено ли управление через веб-интерфейс?
    "playWavEngineId": "audioplayer", # плагин проигрыша WAV-файлов. Некоторые WAV требуют sounddevice.
    "replyNoCommandFound": "Извини, я не поняла", # ответ при непонимании
    "replyNoCommandFoundInContext": "Не поняла...", # ответ при непонимании в состоянии контекста
    "replyOnlineRequired": "Нужен онлайн", # ответ при вызове в оффлайн функции плагина, требующего онлайн 
    "tempDir": "temp", # папка для временных файлов
    "ttsEngineId": "pyttsx", # используемый TTS-движок
    "ttsEngineId2": "", # 2 используемый TTS-движок. Работает только на локальную озвучку - например, буфера обмена. Вызывается командой say2
    "useTTSCache": false, # при установке true в папке tts_cache будет кэшировать .wav файлы со сгенерированными TTS-движком ответами
    "v": "1.7", # версия плагина core. Обновляется автоматически, не трогайте
    "voiceAssNames": "ирина|ирины|ирину" # Если это появится в звуковом потоке, то дальше будет команда. (Различные имена помощника, рекомендуется несколько)
}
```

### Отладка и разработка (для разработчиков)

Для отладки можно использовать запуск системы через файл **runva_cmdline.py**. 

Она делает запуск ядра (**VACore in vacore.py**) через интерфейс командной строки, это удобнее, чем голосом диктовать.

* Подключить собственный навык можно, создав плагин в **plugins_**. Смотрите примеры.
* Подключить собственный TTS можно плагином. Как примеры, смотрите plugins_tts_console.py, plugins_tts_pyttsx.py.
* Также, создав собственный **runva_** файл, можно, при желании, подключить свойт Speech-To-Text движок.

### Разработка плагинов

[Документация по разработке](/docs/DEV_PLUGINS.md)

## Удаленная работа (сервер-клиент, мультимикрофонные/машинные инсталляции)

Мультиинсталляция в режиме "клиент-сервер" несколько сложнее, но позволяет управлять Ириной:
- с нескольких микрофонов
- с разных машин
- из Телеграма (с помощью телеграм-бота)

[Подробнее про настройку клиент-серверного режима](/docs/INSTALL_MULTI.md)

## Speech-to-Text через VOSK remote

Если у вас проблемы с установкой VOSK (например, на Mac), то вы можете воспользоваться 
работой через VOSK Auto Speech Recognition Server, который запускается через Докер.

- Запустите `docker run -d -p 2700:2700 alphacep/kaldi-ru:latest` 
(детали: https://alphacephei.com/vosk/server )
  - или как вариант, вы можете запустить `vosk_asr_server.py`, переопределив внутри параметры

```python
    args.interface = os.environ.get('VOSK_SERVER_INTERFACE', "0.0.0.0")
    args.port = int(os.environ.get('VOSK_SERVER_PORT', 2700)
```  

- Запустите `runva_voskrem.py`. Он будет читать данные с микрофона и отправлять на сервер 
для распознавания.

В случае, если надо запустить распознавание на другой машине -
используйте параметр -u (--uri): `runva_voskrem.py -u=ws://100.100.100.100:2700` 
для уточения адреса сервера.

## Speech-to-Text через SpeechRecognition

SpeechRecognition - классический движок для запуска распознавания через Google и ряд других сервисов.
Для запуска этого распознавания запустите систему через файл **runva_speechrecognition.py**.

Для работы потребуется:

`pip install PyAudio`

`pip install SpeechRecognition`

Если есть проблемы с установкой PyAudio, прочтите детали [у EnjiRouz](https://github.com/EnjiRouz/Voice-Assistant-App/blob/master/README.md)

**Особенности:** распознавание числительных. Одна и та же фраза распознается так:
* VOSK: таймер десять секунд
* SpeechRecognition (Google): таймер 10 секунд

## Поддержка многоязычности
Проект в целом не предполагает поддержки многоязычности, т.к. использует кастомный парсинг слов в плагинах. 
Но, тем не менее, ядро (**vacore.py**) совершенно не привязано к языку, и вы можете собрать собственную инсталляцию на другом языке, просто переписав для них плагины.

Несколько языковых фраз, определяющих core-поведение языкового помощника (его имя, а также фразы типа "Я не поняла")
настраиваются в файле конфигурации плагина **core**.

## Нечеткая обработка фраз

C версии 7.5 поддерживает нечеткую обработку пользовательского ввода.
Пока эффективных плагинов этого типа нет.
Пример реализации плагина - plugins_inactive/plugin_fuzzy_demo.py

Для задания порога распознавания есть глобальный параметр fuzzyThreshold в core.json, 
он принимает значения от 0 до 1 (1 - полная уверенность в фразе)

Два известных плагина, работающих с этим:
* https://github.com/janvarev/irene_plugin_fuzzy_thefuzz - через thefuzz, нечеткое сравнение строк
* https://github.com/modos189/irene_plugin_fuzzy_sklearn - через scikit-learn

## Плагины от голосового помощника Васисуалия

С версии 8.1 в тестовом режиме сделана поддержка core-плагинов от голосового помощника Васи:
https://github.com/Oknolaz/vasisualy

Для добавления:
1. Плагины надо кидать в plugins_vasi/skills (брать в https://github.com/Oknolaz/vasisualy/tree/master/vasisualy/skills )
2. от каждого плагина ожидается, что в модуле будет прописан triggers, на основании которого
формируется список команд. Если нет - плагин надо доработать.

Работает в простейших случаях - протестировано на плагинах coin и crystall_ball.

Если не работает - читайте код. Поддержка сделана через плагин plugin_vasi.py.



## Contributing

Если вы хотите что-то добавить в проект, хорошо ознакомиться с
Политикой 
[CONTRIBUTING.md](/CONTRIBUTING.md)

Коротко:
* Под плагины желательно делать отдельные Github-проекты (или размещать их где-то еще), которые вы готовы поддерживать. Ссылки можно кидать в [https://github.com/janvarev/Irene-Voice-Assistant/issues/1](https://github.com/janvarev/Irene-Voice-Assistant/issues/1), чтобы ваш плагин нашли другие. Кидать дополнительные плагины в этот проект не нужно - у меня нет времени и сил поддерживать то, в чём я не разбираюсь.
* Делайте точечные изменения, улучшающие функциональность или фиксящие баги (например, нерабочесть в каких-то условиях). Такие Pull Request с высокой вероятностью будут приняты.
* Массовые изменения кода (приведения стиля кода к единому, организация импортов) **не будут рассматриваться и будут отклонены**. Пожалуйста, не делайте их.

## Благодарности

@EnjiRouz за проект голосового ассистента: https://github.com/EnjiRouz/Voice-Assistant-App, который стал основой (правда, был очень сильно переработан)

AlphaCephei за прекрасную библиотеку распознавания Vosk ( https://alphacephei.com/vosk/index.ru ) 

## Поддержка проекта

Основная сложность в опенсорс - это не писать код. Писать код интересно.

Сложность в опенсорс - поддерживать код и пользователей в течение долгого времени.

Отвечать на вопросы. Фиксить баги. Писать статьи и документацию.

Если вы хотите поддержать мой интерес и сделать так, чтобы Ирина, 
как независимый от больших компаний голосовой помощник долго, поддерживалась, вы можете:

- **Написать новый плагин** (меня всегда это радует!)
- **Закинуть денежку** через подписку в https://boosty.to/irene-voice Чем больше у меня подписчиков, тем лучше я понимаю, что проект нужен.
- Рассказать кому-то об Ирине, или помочь её настроить.
- Просто сказать "спасибо" в этой ветке: https://github.com/janvarev/Irene-Voice-Assistant/issues/12


  


