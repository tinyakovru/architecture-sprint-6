# Проблемы и риски

- "узким местом" описанной системы является синхронный запрос, внутри которого выполняется несколько других синхронных запросов и полученные данные агрегируются. Приложение, выполняющее запрос, зависит от скорости работы сторонних сервисов и в случае сбоя любого из них запрос будет выполняться до момента обрыва по таймауту.
- данный запрос занимает ресурсы и мешает приложению обрабатывать запросы от пользователей
- с увеличением количества интеграций со страховыми компаниями ситуация будет только усугубляться

# Решение 
В качестве решения предлагается перейти на EDA при интеграции сервисов core-app - ins-product-aggregator  и ins-comp-settlement - ins-product-aggregator

При этом, использовать паттерн Transactional Outbox, на мой взгляд, не имеет смысла, т.к. этот паттерн используется для гарантии оповещения подписчиков о изменении данных в БД. В нашем же случае записи в БД нет: сервис ins-product-aggregator получает данные  через REST у партнерских страховых и публикует их в брокер сообщений, откуда они вычитываются подписчиками.
