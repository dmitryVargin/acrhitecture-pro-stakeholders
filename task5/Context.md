@startuml context_diagram_credit
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

LAYOUT_LEFT_RIGHT()

title  Онлайн-подача заявок на кредит диаграмма контекста  

Person(client, "Клиент", "Подает заявку на кредит онлайн")

Person(call_center_manager, "Менеджер кол-центра", "Звонит клиентам с одобренными заявками для назначения встречи")
Person(front_office_manager, "Менеджер фронт-офиса", "Проводит идентификацию и выдает кредиты в отделении")
Person(credit_manager, "Менеджер бэк-офиса кредитов", "Обрабатывает одобренные заявки в кредитном конвейере")
System(banking_system, "Банковская система 'Стандарт'", "Обеспечивает подачу и обработку кредитных заявок")
System_Ext(credit_bureau, "Бюро кредитных историй", "Предоставляет кредитные истории клиентов для скоринга")
System_Ext(sms_gateway, "СМС-шлюз телеком-оператора", "Отправка СМС-уведомлений клиентам")

Rel(client, banking_system, "HTTPS/Веб-сайт, Интернет-банк")
Rel(banking_system, credit_bureau, "REST API")
Rel(banking_system, sms_gateway, "Отправляет запросы на СМС-уведомления")
Rel(sms_gateway, client, "SMS")
Rel_U(banking_system, call_center_manager,)
Rel_U(call_center_manager, client,)
Rel(banking_system, credit_manager,)
Rel(credit_manager, banking_system,)
Rel(client, front_office_manager,)
Rel(front_office_manager, banking_system,)

@enduml