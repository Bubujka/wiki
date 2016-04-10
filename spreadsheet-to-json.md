# Как сделать управление текстами на сайте через google spreedsheet


## В общих чертах
- Создаём таблицу в google-spreadsheet
- Составляем команду для скачивания и преобразования таблицы в json
- Интегрируем json в сайт

## Предварительно

На компьютере должен быть установлен node.js

Нужно установить две зависимости глобально:
```sh
npm install -g google-spreadsheet-to-json pac
```

Они предоставят две команды
- gsjson
- pac

## Создаём новую таблицу

Заходим в [гуглотаблицы](https://docs.google.com/spreadsheets/u/0/)

Выбираем пустой шаблон ![img](http://ss.bubujka.org/2016/04/10/14-50-55_17d9e8c4b9.png)

Даём ей название и заполняем данными ![img](http://ss.bubujka.org/2016/04/10/14-54-29_28dca15b93.png)

Открываем меню **File** и выбираем пункт **Publish to the web...** ![img](http://ss.bubujka.org/2016/04/10/14-55-51_e08d6af62b.png)

Просто жмём на кнопку publish  ![img](http://ss.bubujka.org/2016/04/10/14-58-26_1cd8d4aa8f.png)

Из адресной строки копируем номер документа
![img](http://ss.bubujka.org/2016/04/10/14-59-18_b598dfdf8f.png)

У меня он в данном случае **1FCQfnHkLzoIQLB39vfloEVZ5dkyy38T-QBgRFpx3HtI**

Проверяем в консоли что всё сделано правильно:
```sh
gsjson -b $DOCUMENT_ID test.json ; cat test.json
```

У меня получилось следующее:
![img](http://ss.bubujka.org/2016/04/10/15-02-40_d897b77333.png)

## Алиас для команды

Чтобы не запоминать длинный номер документа, лучше занести его в package.json

Добавляем строчку в раздел scripts:
![img](http://ss.bubujka.org/2016/04/10/15-14-55_891269008a.png)

После этого можно вызывать данную команду с помощью **npm run**
![img](http://ss.bubujka.org/2016/04/10/15-16-23_d3715ef388.png)

## Интеграция с harpjs

harp.js специальным образом обрабатывает файл _data.json, поэтому сохранять данные нужно именно в этот файл
![img](http://ss.bubujka.org/2016/04/10/19-10-27_894331f120.png)

В коде проекта они доступны будут через объект `public._data`. К примеру:
![img](http://ss.bubujka.org/2016/04/10/19-11-19_01a5142854.png)

## Интеграция с webpack

Нужно установить [json-loader](https://github.com/webpack/json-loader)
```sh
npm install --save-dev json-loader
```

И внести его в конфиг webpack.config.js:
```js
{
module:{
  loaders: [
    { test: /\.json$/, loader: 'json' }
  ]
}
```

После этого можно делать `require('./txt.json')`

## Интеграция с jekyll

Документация на эту тему: [https://jekyllrb.com/docs/datafiles/](https://jekyllrb.com/docs/datafiles/)

Сперва нужно создать каталог `_data`
```sh
mkdir _data
```

Json файл должен располагаться именно в этой папке
![img](http://ss.bubujka.org/2016/04/10/19-19-08_bef0574b30.png)

Доступны данные будут внутри объекта `site.data.txt`
![img](http://ss.bubujka.org/2016/04/10/19-21-18_00d1d14cf1.png)



## Перед коммитом

Нужно упаковать необходимые зависимости:

```sh
npm install --save-dev google-spreadsheet-to-json
pac
```


## Варианты использования gsjson

Значительно лучше у самого проекта написано: [google-spreadsheet-to-json](https://github.com/bassarisse/google-spreadsheet-to-json)

Но вот что самое главное надо знать

### Без аргументов

```sh
gsjson $DOCUMENT_ID test.json
```
![img](http://ss.bubujka.org/2016/04/10/15-04-42_374f9df306.png)

### Добавить отступы -b

```sh
gsjson -b $DOCUMENT_ID test.json
```
![img](http://ss.bubujka.org/2016/04/10/15-05-57_f788d556f0.png)

### Использовать одно из полей как ключ -c $FIELD

```sh
gsjson -b -c $FIELD $DOCUMENT_ID test.json
```
![img](http://ss.bubujka.org/2016/04/10/15-07-46_fdd4159103.png)

## Тонкости

Google spreadsheet обновляет документ один раз в 5 минут.

Если нет файла `package.json`, то создать его можно командой `npm init`


