# Описание стандарта взаимодействия между «1С:Предприятие 8» и банковским сервисом по технологии DirectBank (прямой обмен с банком)

Прямой обмен электронными документами из решений «1С:Предприятие 8» с банковской состоит из двух уровней взаимодействия:

1. Прикладной уровень – описывает структуру и порядок взаимодействия с прикладными объектами, которые участвуют в обмене.

2. API обмена данными – уровень который описывает подходы и методы осуществления операций обмена данными, он в свою очередь делится делится на следующие уровни:
 - Уровень аутентификации;
 - Транспортный уровень.

Так же имеется возможность организации обмена с банком используя внешнюю компоненту.

Графически это можно отобразить так:

![](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/doc_imgs/level_description.png)


+ [Прикладной уровень](https://github.com/1C-Company/DirectBank/blob/master/doc/application-layer/readme.md)
+ [API прямого обмена данными с банком](https://github.com/1C-Company/DirectBank/blob/master/doc/transport-api/readme.md)
+ [API прямого обмена данными с банком через внешнюю компоненту](https://github.com/1C-Company/DirectBank/blob/master/doc/vk-transport-api/readme.md)
+ [Схемы данных](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md)
