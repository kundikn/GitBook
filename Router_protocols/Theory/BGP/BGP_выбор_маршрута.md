# BGP выбор маршрута 

***BGP** анонсирует не все приходящие маршруты, а только лучшие.*

Итак, критерии выбора лучших:

- Максимальное значение **Weight** (локально для маршрутизатора, только для **Cisco**) - это один из первых атрибутов, которые рассматриваются при выборе лучшего маршрута в **BGP**

- Максимальное значение **Local Preference** (Атрибут для внутренней **AS**) - определяет, через какой пограничный маршрутизатор пакет должен покинуть **AS**

- Предпочесть локальный маршрут маршрутизатора (**next hop = 0.0.0.0**)

- Кратчайший путь через автономные системы. (самый короткий **AS_PATH**)

- Минимальное значение **Origin Code** (**IGP** (*network...*) > **EGP** (*устаревший протокол*) > **incomplete** *(от других проктоколов, типа **OSPF***))

- Минимальное значение **MED** (*распространяется между автономными системами*) - помогает внешним автономным системам (**AS**) понять, через какой пограничный маршрутизатор предпочтительно входить в вашу сеть.

- Путь **eBGP** лучше чем путь **iBGP**

- Выбрать путь через ближайшего **IGP-соседа** - *если это условие выполнено, то происходит балансировка нагрузки между несколькими равнозначными линками.*

  

