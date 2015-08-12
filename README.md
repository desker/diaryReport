# diaryReport
Формирование отчетов (из JSON в PNG) с помощью fabric.js.

##Описание входящих данных

Функции diaryReport(jsonObject) на вход будут приходить JSON следующего формата.

``` json
  {
    "items": [
      {
        "type": "type_of_item",
        "data": {"Данные":1}
      },
      {"..."}
    ]
  }

```

*items* - Массив строк, из которых будет строится отчет.

*items.type* - Тип строки.

*items.data* - Данные необходимые для отрисоки данной строки. Это может быть объект, строка, массив, число  и т.п.

Функционал каждой строки реализовать в отдельном js файле.

Итоговая сборка должна создать один минифицированный файл (umd).



###Тип строки - **header**

Данный тип строки служит для отображения заголовка отчета.

``` json
    {
      "type": "header",
      "data": "Женский комплекс упражнений 'три в одном'"
    }
```

![](http://storage-145851-1.cs.clodoserver.ru/test/header.png)

###Тип строки - **subHeader**

Заголовок 2 уровня.

``` json
    {
      "type": "subHeader",
      "data": "Первая тренировка   11:00 - 12:00"
    }
```

![](http://storage-145851-1.cs.clodoserver.ru/test/subHeader.png)

###Тип строки - **text**

Абзац с текстом. 

*maxWidth* - Максимальная ширина абазаца.

``` json
    {
      "type": "text",
      "data": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean euismod bibendum laoreet. ...",
      "maxWidth": 400
    }
```

![](http://storage-145851-1.cs.clodoserver.ru/test/blockList.png)


###Тип строки - **blockList**

Список в блоке. 

*title* - Заголовок блока.

``` json
    {
      "type": "blockList",
      "title": "Итоги за день",
      "data": [
        "Энергозатраты: 80 ккал",
        "Количество повто......"
      ]

    }
```

![](http://storage-145851-1.cs.clodoserver.ru/test/blockList.png)


###Тип строки - **diaryExercise**

Упражнение в дневнике тренировок. 

*data.name* - Наименование упражнения.

*data.count* - Количество колонок.

*data.groups* - Группировка колонок. Не обязательный параметр. Если не передано, то колнкине группируются.

*data.outcomeSport* - Энергозатраты. Если не передано или равно 0, то не выводится.

*data.rows* - Строки описывающие упражнение.

*data.rows[].title* - Наименование первой колонки. Ширина колонки равна 210px

*data.rows[].values* - Значения для заполнения ячеек. Может быть так, что не для всех ячеек будут переданы значения. Т.е. они просто остануться пустыми.

*data.rows[].values[].cell* - Номер ячейки 

*data.rows[].values[].value* - Значение подставляемое в ячейку.
Ширина ячейки 50px

``` json
    {
      "type": "diaryExercise",
     
      "data": {
        "name": "Разгибание рук в наклоне",
        "count": 10,
        "groups": [
          {"start": 1, "end": 3},
          {"start": 4, "end": 6},
          {"start": 7, "end": 8}
        ],
        "outcomeSport": 25,
        "rows": [
            {
            "title": "Количество повторений", 
            "values": [{"cell": 1, "value": "10"}], {"cell": 4, "value": "9"}] 
            },
            
             {
            "title": "Вес снаряда", 
            "values": [{"cell": 1, "value": "30"}], {"cell": 4, "value": "30"}] 
            }
        ]
      }

    }
```


###Тип строки - **diarySuperSet**

Суперсет в дневнике тренировок. Это объединение несколких упражнений в один блок.



``` json
    {
      "type": "diarySuperSet",

      "data": {
        "title": "Суперсет",
        "count": 10,
        "groups": [
          {"start": 1, "end": 3},
          {"start": 4, "end": 6},
          {"start": 7, "end": 8}
        ],
        "outcomeSport": 25,
        "exercise": [
            {
              "см. описание типа строки diaryExercise":1
            }
        
        ]
      }

    }
```

![](http://storage-145851-1.cs.clodoserver.ru/test/superset.png)



![](http://storage-145851-1.cs.clodoserver.ru/test/diaryExercise.png)

###Тип строки - **sportProgramDay**

Представление дня программы тренировок. 

*data.name* - Наименование дня программы тренировок.

*data.rows* - Строки описывающие день программы тренировок.

*data.rows[].title* - Наименование упражнения

*data.rows[].value* - План программы

*data.rows[].number* - Номер строки. Если пустой, то выводить не нужно.

``` json
    {
      "type": "sportProgramDay",
     
      "data": {
        "name": "2. Вторая тренировка (руки)",
    
        "rows": [
            {
            "title": "Скручивание на полу (ноги вверху)", 
            "value": "4x12-20",
            "number": "1"
            },
            
            {
            "title": "Становая тяга классическая", 
            "value": "4x8-12",
            "number": ""
            },
            "..."
        ]
      }

    }
```

![](http://storage-145851-1.cs.clodoserver.ru/test/sportProgramDay.png)


