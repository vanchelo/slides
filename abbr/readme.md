# Деконструкция аббревиатур

Здравствуйте, меня зовут Дим. А вы на канале "Core Dump", где мы берём разные темы из компьютерной науки и деконструируем их по полочкам.

> **TDD** SRP OCP LSP ISP DIP SOLID DRY KISS

Вы можете [открыть в интерфейсе проведения презентаций](https://nin-jin.github.io/slides/abbr/).

## Деконструкция TDD

Начнём мы с разработки через тестирование.

> Test Driven Development

Суть этого подхода заключается в ритуализации процесса разработки. То есть в некритическом безусловном выполнении определённых простых действий. Этот ритуал сделает ваш код красивым, легко поддерживаемым и содержащим минимум дефектов. Так во всяком случае настоятельно убеждают нас проповедники TDD.

### Суть TDD

Вкратце, ритуальный цикл состоит из 3 шагов: сначала пишется красный тест; потом пишется или исправляется код так, чтобы тест позеленел; и наконец код рефакторится, сохраняя зеленоватость тестов.

![Pure TDD](https://habrastorage.org/webt/m7/wm/ts/m7wmts_olawooeqs5kcduncwgyy.png)

И тут сразу возникает вопрос впрос на миллион...

### Что делать, когда тест изначально зелёный?

Варианты ответов...

- Ломать код
- Удалять тест
- Это невозможно

А правильный ответ - все эти варианты не верные. Хотя именно один из них как правило слышишь от адептов TDD. И наоборот, подобные ответы редко можно услышать от тех, кто TDD не практикует.

### Парадокс воронов

Тут нельзя не упомянуть про парадокс воронов. Суть его в том, что задаётся вопрос: "Все ли вороны чёрные?". И для ответа на него берутся нечёрные предметы. Например - красные яблоки. И каждое такое яблоко как бы подтверждает тезис о том, что "все вороны чёрные", ведь яблоки не чёрные и при этом не вороны.

![](https://habrastorage.org/webt/uq/lh/ri/uqlhriwwnnffs1cqlkzmxhjczbi.jpeg)

Так же как в парадоксе воронов, адепт TDD нередко думает, что падение теста на явно некорректном коде может хоть что-то сказать о том, будет ли тест падать на коде, похожем на корректный. Однако, тут нет никакогой связи. Это просто два разных кода. Поломка и починка кода - это не более чем бессмысленное действие, необходимое только для того, чтобы форманьно пройти 2 обязательных шага ритуального цикла. Но насколько вероятна эта зелёная ситуация?

### Иначально зелёные тесты неизбежны

Даже для самой простой функции вам не хватит одного теста. Ведь надо рассмотреть позитивные и негативные сценарии, граничные условия, классы эквивалентности. Используя TDD вы сможете написать первый красный тест, другой, третий, но довольно быстро вы столкнётесь с ситуацией, что вы уже написали код, который удовлетворяет всем тест-кейсам, но собственно тестов вы написали меньше половины.

1. ❌ ⇝ ✅
2. ❌ ⇝ ✅
3. ❌ ⇝ ✅
4. ✅ ❓
5. ✅ ❓
6. ✅ ❓
7. ✅ ❓
8. ✅ ❓

Если мы оставим такие тесты, то это уже будет не TDD, ведь мы написали большую часть тестов уже после кода, а не до. Если же мы их удалим, то код останется попросту недотестированным. И на ближайшем рефакторинге мы скорее всего допустим дефект, который не будет обнаружен ввиду отсутствия теста для соответствующего кейса.

То есть, для обеспечения качества мы вынуждены явно нарушать основную идею TDD: сначала тест, потом код.

### Правильный TDD

Тем не менее, давайте попробуем представить, как мог бы выглядеть TDD здорового человека...

![Fixed TDD](https://habrastorage.org/webt/it/vp/ky/itvpky8gu9nq4_iiyisql-_n6zi.png)

Первым делом мы должны задаться вопросом "А нужен ли нам ещё один тест?". Если мы ещё не покрыли тестами все тест кейсы, значит нужен. И следующим шагом мы пишем тест. Потом проверяем проходит ли он. И если проходит, то возвращаемся на исходную. Если же не проходит, то это значит, что тест и реализация не соответствуют друг другу и нам надлежит это починить. Починка заключается либо в исправлении кода, либо исправлении теста. Проблема может быть и там, и там. После внесения изменений мы слова прогоняем тесты. И если всё зелёное, то возвращаемся на начало цикла. И вот только когда мы уже покрыли все кейсы тестами и больше тестов нам не надо, только тогда мы начинаем рефакторинг. Если в процессе рефакторинга тесты начнут падать, то нужно будет вернуться к этапу починки. И, наконец, обратите внимание, что даже после рефакторинга мы задаёмся вопросом "А нужен ли ещё один тест?". Но как же так, - спросите вы, - мы же уже покрыли все тест-кейсы. Причина тут в том, что рефакторинг может приводить к существенному изменению кода. А это в свою очередь может изменить граничные условия, для учёта которых могут потребоваться дополнительные тесты.

В такой форме TDD уже можно применять с пользой. Однако...

### TDD приводит к куче лишней работы

TDD предлагает нам даже не пытаться писать полнофункциональную реализацию, ограничившись лишь минимальной реализацией достаточной для прохождения очередного теста. Следовательно мы зачастую будем писать явно некорректный код, который на следующий итерации будет отправляться в трубу.

Давайте рассмотрим типичный сценарий написания простой функции...

| Итерация | В начале        | В процесссе     | В результате
|----------|-----------------|-----------------|--------------------------
| 1        | ❌             | ❌              | ✅
| 2        | ✅❌           | ❌❌           | ✅✅
| 3        | ✅✅❌        | ❌❌❌         | ✅✅✅
| 4        | ✅✅✅❌      | ✅✅❌❌      | ✅✅✅✅
| 5        | ✅✅✅✅❌   | ✅✅✅✅❌    | ✅✅✅✅✅
| 6        | ✅✅✅✅✅❌ | ❌❌❌❌❌❌ | ✅✅✅✅✅✅

Первые несколько итераций мы выкидываем ранее написанный код полностью. Это приводит к покраснению воообще всех тестов, после чего нам приходится уже писать код так, чтобы он проходил сразу несколько тестов. То есть мы приходим к ситуации, как если бы мы сразу написали несколько тестов, и только после этого взялись писать код. А все предыдущие итерации получается не имели никакого смысла.

Но всё не так плохо, с какого-то момента мы перестанем выбрасывать код полностью, и добавление нового теста уже не будет приводить нас к существенному переписыанию кода. То есть цикл TDD наконец-то начинает работать с пользой.

Однако, всё ещё остаётся довольно высокая вероятность в любой момент столкнуться с такой ситуацией, что очередное изменение кода сломает вообще все тесты или как минимум большую их часть. А значит мы снова возвращаемся к ситуации "все тесты уже написаны, нужно лишь написать правильно код". А все те осторожные шажки по одному тесту просто обесцениваются.

### Когда TDD полезен

Как было отмечено ранее применение TDD в общем случае скорее вредно, чем полезно. Но в некоторых случаях оно может быть оправдано..

- Исправление дефектов
- Заранее известный контракт
- Не заставить себя писать тесты

### Программировать ли по TDD?

Если кто-то вам скажет, что он "программирует по TDD", то можете быть уверены, что он попросту не ведает, что творит. У TDD есть ряд фундаментальных проблем, поэтому его применение оправдано лишь в весьма ограниченном числе случаев. И то, не в той форме в которой ему как правило учат многочисленные коучи.

- Ритуализация ❌
- Там, где это уместно ✅
- Не зацикливаться ✅

# Продолжение следует..

- **Лайк**, если материал оказался вам полезен.
- **Подписка**, чтобы не пропустить новые материалы.
- **Комментарий**, если есть что добавить или опровергнуть.
- **Поделиться**, чтобы приобщиись и другие.