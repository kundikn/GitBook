# EIGRP (Enhanced Interior Gateway Routing Protocol)

**EIGRP (Enhanced Interior Gateway Routing Protocol)** -это усовершенствованный протокол внутреннего шлюза маршрутизации, разработанный компанией Cisco.

Итак, чем хорош **EIGRP**? Быстрое переключение на заранее просчитанный запасной маршрут, а также поддерживает балансировку нагрузки по равной и неравно стоимости.
Например, существует показатель который мы задаем вручную (по дефолту он равен 1), и если мы его изменим на 2 (диапозон от 1 до 128), то будут использвоаться маршруты, не превыщающие 2 раза основной маршрут.

---------

## Метрика

Для оценки качества определенного маршрута, в протоколах маршрутизации используется некое число, отражающее различные его характеристики или совокупность характеристик - **метрика**. Характеристики, принимаемые в расчет, могут быть разными- начиная от количества роутеров на данном маршруте и заканчивая средним арифметическим загрузки всех интерфейсов по ходу маршрута. Что касается метрики **EIGRP**, процитируем Jeremy Cioara: 

> У меня создалось впечатление, что создатели EIGRP, окинув критическим взглядом свое творение, решили, что все слишком просто и хорошо работает. И тогда они придумали формулу метрики, что бы все сказали “ВАУ, это действительно сложно и профессионально выглядит.

Узрите же полную формулу подсчета метрики EIGRP:
$$
(K1 bw + (K2 bw) / (256 — load) + K3 delay) (K5 / (reliability + K4))
$$
в которой:

- **bw** это не просто пропускная способность, а (*10000000/самая маленькая пропускная способность по дороге маршрута в килобитах*)
- **delay** это не просто задержка, а **сумма всех задержек** по дороге в десятках микросекунд (**delay** в командах **show interface**, s**how ip eigrp topology** и прочих показывается *в микросекундах*!)
- **K1-K5** это коэффициенты, которые служат для того, чтобы в формулу “*включился*” тот или иной параметр.

Страшно? Было бы, если бы все это работало, как написано. На деле же из всех 4 возможных слагаемых формулы, по умолчанию используются только два: **bw** и **delay** (коэффициенты **K1** = **K3 **= **1**, остальные нулю), что сильно ее упрощает — мы просто складываем эти два числа (не забывая при этом, что они все равно считаются по своим формулам). Важно помнить следующее: *метрика считается по худшему показателю пропускной способности по всей длине маршрута*.
Если K5=0, то используется следующая формула: 
$$
Metric = (K1 bw + (K2 bw) / (256 — load) + (K3 * delay)
$$

-----------

## Работа EIGRP

### FD and AD

Определимся с терминами, применяемыми внутри **EIGRP**. Каждый маршрут в **EIGRP** характеризуется двумя числами: **Feasible Distance** и **Advertised Distance** (вместо **Advertised Distance** иногда можно встретить **Reported Distance**, это одно и то же). *Каждое из этих чисел представляет собой метрику, или стоимость* (чем больше-тем хуже) данного маршрута с разных точек измерения: 

- **FD** это “*от меня до места назначения*” 
- **AD**- “*от соседа, который мне рассказал об этом маршруте, до места назначения*”. 

Ответ на закономерный вопрос “Зачем нам надо знать стоимость от соседа, если она и так включена в **FD**?”- чуть ниже (пока можете остановиться и поломать голову сами, если хотите).

### Successor and Feasible Successor

У каждой подсети, о которой знает **EIGRP**, на каждом роутере существует **Successor- роутер** из числа соседей, *через который идет лучший (с меньшей метрикой), по мнению протокола, маршрут к этой подсети*. Кроме того, *у подсети может также существовать один или несколько запасных маршрутов* (роутер-сосед, через которого идет такой маршрут, называется **Feasible Successor**). 

> **EIGRP**- единственный протокол маршрутизации, запоминающий запасные маршруты (в **OSPF** они есть, но содержатся, так сказать, в “сыром виде” в таблице топологии- их еще надо обработать алгоритмом **SPF**)

... что дает ему плюс в быстродействии: как только протокол определяет, что основной маршрут (через **successor**) недоступен, он сразу переключается на запасной. Для того, чтобы роутер мог стать **feasible successor** для маршрута, его **AD** должно быть меньше **FD successor’а** этого маршрута (*вот зачем нам нужно знать AD*). Это правило применяется для того, чтобы избежать колец маршрутизации.

### Соседство

Роутеры не разговаривают о маршрутах с кем попало — прежде чем начать обмениваться информацией, они должны установить отношения соседства. После включения процесса командой **router eigrp** с указанием номера автономной системы, мы, командой **network** говорим, какие интерфейсы будут участвовать и одновременно, информацию о каких сетях мы желаем распространять. Незамедлительно, через эти интерфейсы начинают рассылаться **hello-пакеты** на мультикаст- адрес 224.0.0.10 (по умолчанию каждые 5 секунд для ethernet). Все маршрутизаторы с включенным **EIGRP** получают эти пакеты, далее каждый маршрутизатор-получатель делает следующее:

- Сверяет адрес отправителя **hello-пакета**, с адресом интерфейса, из которого получен пакет, и удостоверяется, что они из одной подсети
- Сверяет значения полученных из пакета K-коэффициентов (проще говоря, какие переменные используются в подсчете метрики) со своими. Понятно, что если они различаются, то метрики для маршрутов будут считаться по разным правилам, что недопустимо
- Проверяет номер автономной системы
- Опционально: если настроена аутентификация, проверяет соответствие ее типа и ключей

Если получателя все устраивает, он добавляет отправителя в список своих соседей, и посылает ему (уже юникастом) **update-пакет**, в котором содержится список всех известных ему маршрутов (**aka full-update**). Отправитель, получив такой пакет, в свою очередь, делает то же самое. Для обмена маршрутами **EIGRP** использует **Reliable Transport Protocol** (**RTP**, не путать с Real-time Transport Protocol, который используется в ip-телефонии), который подразумевает подтверждение о доставке, поэтому каждый из роутеров, получив **update- пакет**, отвечает **ack** -пакетом (сокращение от **acknowledgement** - **подтверждение**). Итак, отношение соседства установлены, роутеры узнали друг у друга исчерпывающую информацию о маршрутах, что дальше? Дальше они будут продолжать посылать мультикаст **hello-пакеты** в подтверждение того, что они на связи, а в случае изменения топологии - **update-пакеты**, содержащие сведения только об изменениях (**partial update**).

------------

## Вымышленная ситуация

Представим ситуацию: R2 по каким-то причинам потерял связь с 192.168.2.0/24. До этой подсети у него нет запасных маршрутов (т.е. отсутствует **FS**). Как всякий ответственный роутер с **EIGRP**, он хочет восстановить связь. Для этого он начинает рассылать специальные сообщения (**query- пакеты**) всем своим соседям, которые, в свою очередь, не находя нужного маршрута у себя, расспрашивают всех своих соседей, и так далее. Когда волна запросов докатывается до R4, он говорит “*погодите-ка, у меня есть маршрут к этой подсети! Плохонький, но хоть что-то. Все про него забыли, а я-то помню*”. Все это он упаковывает в **reply-пакет** и отправляет соседу, от которого получил запрос (**query**), и дальше по цепочке. Понятное дело, это все занимает больше времени, чем просто переключение на **Feasible Successor**, но, в итоге, мы получаем связь с подсетью.



![img](https://linkmeup.gitbook.io/~gitbook/image?url=http%3A%2F%2Fimg-fotki.yandex.ru%2Fget%2F6419%2F83739833.20%2F0_9e454_30108738_L.jpg&width=768&dpr=4&quality=100&sign=3920e50b&sv=1)

А сейчас опасный момент: может, вы уже обратили внимание и насторожились, прочитав момент про эту веерную рассылку. Падение одного интерфейса вызывает нечто похожее на **широковещательный шторм в сети** (не в таких масштабах, конечно, но все-таки), причем чем больше в ней роутеров, тем больше ресурсов потратится на все эти запросы-ответы. Но это еще пол-беды. 

Возможна ситуация и похуже: представим, что роутеры, изображенные на картинке- это только часть большой и распределенной сети, т.е. некоторые могут находится за много тысяч километров от нашего R2, на плохих каналах и прочее. **Так вот, беда в том, что, послав query соседу, роутер обязан дождаться от него reply**. Неважно, что в ответе - но он должен прийти. **Даже если роутер уже получил положительный ответ, как в нашем случае, он не может поставить этот маршрут в работу, пока не дождется ответа на все свои запросы**. А запросы-то, может, еще где-нибудь на Аляске бродят. Такое состояние маршрута называется **stuck-in-active**. 

Тут нам нужно познакомится с терминами, отражающими состояние маршрута в **EIGRP**: **active\passive route**. Обычно они вводят в заблуждение. Здравый смысл подсказывает, что **active** значит маршрут “*активен*”, включен, работает. Однако тут все наоборот: **passive** это “*все хорошо*”, а состояние **active** означает, что *данная подсеть недоступна*, и маршрутизатор находится в активном поиске другого маршрута, рассылая **query** и ожидая *reply*. Так вот, состояние **stuck-in-active** (*застрял в активном состоянии*) может продолжатся до 3 минут! По истечение этого срока, роутер обрывает отношения соседства с тем соседом, от которого он не может дождаться ответа, и может использовать новый маршрут через R4. 
Как мы можем избежать инфаркта этой ситуации? Выхода два: **суммирование маршрутов и так называемая stub-конфигурация**.

- В **EIGRP** суммирование маршрутов можно проводить на любом роутере. Для иллюстрации, представим, что к нашему многострадальному R2 подключены подсети от *192.168.0.0/24 до 192.168.7.0/24*, что очень удобненько суммируется в *192.168.0.0/21* (вспоминаем binary math). Роутер анонсирует этот суммарный маршрут, и все остальные знают: если адрес назначения начинается на *192.168.0-7*, то это к нему. Что будет происходить, если одна из подсетей пропадет? Роутер будет рассылать query-пакеты с адресом этой сети (конкретным, например, *192.168.5.0/24*), но соседи, вместо того, чтобы уже от своего имени продолжить порочную рассылку, будут сразу в ответ слать отрезвляющие реплаи, мол, это твоя подсеть, ты и разбирайся.

- Второй вариант- **stub - конфигурация**. Образно говоря, **stub** означает “*конец пути*”, “*тупик*” в **EIGRP**, т.е., чтобы попасть в какую-то подсеть, не подключенную напрямую к такому роутеру, придется идти назад. Роутер, сконфигурированный, как **stub**, *не будет пересылать трафик между подсетями*, которые ему стали известны от **EIGRP** (проще говоря, которые в **show ip route** помечены буквой **D**). Кроме того, его соседи не будут отправлять ему **query-пакеты**. Самый распространенный случай применения- **hub-and-spoke** топологии, особенно с избыточными линками. 

  > То есть, как я понял, просто роутер можно определить как stub, и он не будет информировать о маршрутах, типа если хочешь в ту подсеть иди обратно, что кратно уменьшает число запросов.
  > Аналог passive-interface, но в отличие от него, stub польностью полностью отключает протокол на выбранном интерфейсе, что не позволяет установить соседство.

----------
