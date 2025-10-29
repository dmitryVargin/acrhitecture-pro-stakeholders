```puml
@startuml Container_KC_Rates
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml



title  Передача ставок в Кол-центр  диаграмма Контейнеров 

Person(cc_manager, "Сотрудник Кол-центра")
  
Container(rates_service, "Сервис Ставок", "Java/Spring Boot, PostgreSQL, Redis", "Предоставляет актуальные ставки по депозитам.")



Container(scc_backend, "Система кол-центра (Бэкенд)", "Java Spring Boot")
Container(scc_frontend, "Система кол-центра (Фронтенд)", "React.js", "Интерфейс оператора КЦ.")
Container(abs, "АБС", "Delphi + Oracle DB", "Основная учетная система, мастер-данные по ставкам.")


System(partner_cc, "Система Партнерского КЦ", "Внешняя система, принимает файл")

Rel(cc_manager, scc_frontend, "Использует")
Rel(scc_frontend, scc_backend, "Запрос данных")

Rel(scc_backend, rates_service, "Запрос актуальных ставок", "REST API/SFTP")
Rel(rates_service, abs, "Синхронизация мастер-данных ставок", "Асинхронно")

Rel_Right(rates_service, partner_cc, "Передает файл со ставками (CSV/XLS)", "SFTP")


@enduml
```