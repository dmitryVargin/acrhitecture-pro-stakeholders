```puml
@startuml Context_KC_Rates
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

title Передача ставок в Кол-центр- диаграмма Контекста  

Person(user, "Клиент", "Звонит в кол-центр")
Person(partner_manager, "Сотрудник Партнерского кол-центра", "Консультирует по ставкам.")
Person(cc_manager, "Сотрудник Кол-центра", "Консультирует по ставкам.")


System(rates_system, "Сервис Ставок", "Единый источник правды по депозитным ставкам.")


System(scc, "Система Кол-центра", "Внутренняя платформа для операторов.")
System_Ext(partner_cc, "Система Партнерского кол-центра", "Внешняя платформа Партнера.")
System(abs, "Автоматизированная Банковская Система (АБС)",)

Rel(user, scc,)
Rel(user, partner_cc,)

Rel(cc_manager, scc,)
Rel(partner_manager, partner_cc,)

Rel(scc, rates_system,)
Rel(rates_system, partner_cc, )

Rel_Right(rates_system, abs,)

@enduml
```