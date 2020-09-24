# What was discussed in an interview with EPAM. (september 2020)

![](epam_intro.png)

### Что нового появилось в Java 8?
<details><summary>Ответ</summary>

___

Из всех нововведений, появившихся в Java 8, нужно отметить самые важные:
стримы, лябмды и дефолтные/функциональные методы в интерфейсах. <br>
Будет большим плюсом, если к этому прибавить такие фичи, как :
* Ссылки на методы и конструкторы -> Integer::valueOf, System.out::print
* Функциональные интерфейсы -> Предикаты (Predicate), Функции (Function), Поставщики (Supplier), Потребители (Consumer)
* Опциональные значения (Optional)

*P.S. Если вы знаете, что есть некоторые фичи, но не можете их объяснить, лучше не упоминайте их в ответе.*
</details>


### Что такое функциональный интерфейс? Какие существует стандартные функциональные интерфейсы в Java?
<details><summary>Ответ</summary>

___

Функциональный интерфейс в Java – это интерфейс, который содержит только 1 абстрактный метод. <br>
Основное назначение – использование в лямбда выражениях и method reference. <br>

Стандартные функциональные интерфейсы, появившиеся в Java 8: <br>

* __Predicate<T>__ - проверяет соблюдение некоторого условия. **boolean test(T t);**
* __Consumer<T>__ - выполняет некоторое действие над объектом типа T, при этом ничего не возвращая. **void accept(T t);**
* __Function<T,R>__ - представляет функцию перехода от объекта типа T к объекту типа R. **R apply(T t);**
* __Supplier<T>__ - не принимает никаких аргументов, но должен возвращать объект типа T. **T get();**
* __UnaryOperator<T>__ - принимает в качестве параметра объект типа T, 
выполняет над ними операции и возвращает результат операций в виде объекта типа T. **T apply(T t);**
* __BinaryOperator<T>__ - принимает в качестве параметра два объекта типа T, 
выполняет над ними бинарную операцию и возвращает ее результат также в виде объекта типа T. **T apply(T t1, T t2);**
</details>


### Можно ли объявить в функциональном интерфейсе метод с реализацией?
<details><summary>Ответ</summary>

___

Да, можно. Так как условием функционального интерфейса является наличие одного абстрактного метода, 
он вполне может иметь дефолтные и статические методы.
</details>


### Что такое стрим (Stream API)? Что стрим принимает в параметрах?
<details><summary>Ответ</summary>

___

Stream API — это новый способ работать со структурами данных в функциональном стиле.
Stream (поток) API (описание способов, которыми одна компьютерная программа может взаимодействовать с другой программой) 
— это по своей сути поток данных. Поток представляет последовательность элементов и предоставляет различные методы 
для произведения вычислений над данными элементами.
   
Параметрами стримы принимают функциональные интерфейсы, описанные в предыдущем вопросе.
</details>


### Какие виды операций над стримами вы знаете? Сможете их перечислить?
<details><summary>Ответ</summary>

___

Java Stream API предлагает два вида методов:
1. **Конвейерные** — возвращают другой stream, то есть работают как builder,
2. **Терминальные** — возвращают другой объект, такой как коллекция, примитивы, объекты, Optional и т.д.
<br>
Терминальные операции завершают работу над стримом, после них нельзя применять методы.

##### К конвеерным относятся:

| Метод stream | Описание | Пример  |
|--------------|:--------:|:--------|
| filter | Отфильтровывает записи, возвращает только записи, соответствующие условию |collection.stream().filter(«a1»::equals).count() |
| skip | Позволяет пропустить N первых элементов | collection.stream().skip(collection.size() — 1).findFirst().orElse(«1») |
| distinct | Возвращает стрим без дубликатов (для метода equals) | collection.stream().distinct().collect(Collectors.toList()) |
| map |	Преобразует каждый элемент стрима |	collection.stream().map((s) -> s + "_1").collect(Collectors.toList()) |
| peek | Возвращает тот же стрим, но применяет функцию к каждому элементу стрима | collection.stream().map(String::toUpperCase).peek((e) -> System.out.print("," + e)).collect(Collectors.toList()) |
| limit | Позволяет ограничить выборку определенным количеством первых элементов | collection.stream().limit(2).collect(Collectors.toList()) |
| sorted | Позволяет сортировать значения либо в натуральном порядке, либо задавая Comparator | collection.stream().sorted().collect(Collectors.toList()) |
| mapToInt, mapToDouble, mapToLong | 	Аналог map, но возвращает числовой стрим (то есть стрим из числовых примитивов) | collection.stream().mapToInt((s) -> Integer.parseInt(s)).toArray() |
| flatMap, flatMapToInt, flatMapToDouble, flatMapToLong | Похоже на map, но может создавать из одного элемента несколько | collection.stream().flatMap((p) -> Arrays.asList(p.split(",")).stream()).toArray(String[]::new) |


##### К терминальным относятся: 

| Метод stream | Описание |	Пример |
|--------------|:--------:|:--------|
| findFirst | Возвращает первый элемент из стрима (возвращает Optional) | collection.stream().findFirst().orElse(«1»)
| findAny |	Возвращает любой подходящий элемент из стрима (возвращает Optional) | collection.stream().findAny().orElse(«1»)
| collect |	Представление результатов в виде коллекций и других структур данных | collection.stream().filter((s) -> s.contains(«1»)).collect(Collectors.toList())
| count | Возвращает количество элементов в стриме | collection.stream().filter(«a1»::equals).count()
| anyMatch | Возвращает true, если условие выполняется хотя бы для одного элемента | collection.stream().anyMatch(«a1»::equals)
| noneMatch | Возвращает true, если условие не выполняется ни для одного элемента |	collection.stream().noneMatch(«a8»::equals)
| allMatch | Возвращает true, если условие выполняется для всех элементов |	collection.stream().allMatch((s) -> s.contains(«1»))
| min |	Возвращает минимальный элемент, в качестве условия использует компаратор | collection.stream().min(String::compareTo).get()
| max |	Возвращает максимальный элемент, в качестве условия использует компаратор |	collection.stream().max(String::compareTo).get()
| forEach |	Применяет функцию к каждому объекту стрима, порядок при параллельном выполнении не гарантируется | set.stream().forEach((p) -> p.append("_1"));
| forEachOrdered |	Применяет функцию к каждому объекту стрима, сохранение порядка элементов гарантирует | list.stream().forEachOrdered((p) -> p.append("_new"));
| toArray |	Возвращает массив значений стрима |	collection.stream().map(String::toUpperCase).toArray(String[]::new);
| reduce |	Позволяет выполнять агрегатные функции на всей коллекцией и возвращать один результат |	collection.stream().reduce((s1, s2) -> s1 + s2).orElse(0)

*P.S. У вас не попросят перечислить все методы, но могут спросить любой из них. Поэтому желательно выучить как можно больше.*
</details>


### Какие есть способы создания стримов?
<details><summary>Ответ</summary>

___

| Способ создания стрима | Шаблон создания | Пример |
|------------------------|:---------------:|:-------|
| 1. Классический: Создание стрима из коллекции | collection.stream() | Collection<String> collection = Arrays.asList("a1", "a2", "a3"); <br> Stream<String> streamFromCollection = collection.stream(); |
| 2. Создание стрима из значений | 	Stream.of(значение1,… значениеN) | Stream<String> streamFromValues = Stream.of("a1", "a2", "a3"); |
| 3. Создание стрима из массива | Arrays.stream(массив) | String[] array = {"a1","a2","a3"};<br> Stream<String> streamFromArrays = Arrays.stream(array); |
| 4. Создание стрима из файла (каждая строка в файле будет отдельным элементом в стриме) | Files.lines(путь_к_файлу) | Stream<String> streamFromFiles = Files.lines(Paths.get("file.txt")) |
| 5. Создание стрима из строки | «строка».chars() | IntStream streamFromString = "123".chars() |
| 6. С помощью Stream.builder | Stream.builder().add(...)....build() | Stream.builder().add("a1").add("a2").add("a3").build() |
| 7. Создание параллельного стрима | collection.parallelStream() | Stream<String> stream = collection.parallelStream(); |
| 8. Создание бесконечных стрима с помощью Stream.iterate | Stream.iterate(начальное_условие, выражение_генерации) | Stream<Integer> streamFromIterate = Stream.iterate(1, n -> n + 1) |
| 9. Создание бесконечных стрима с помощью Stream.generate | Stream.generate(выражение_генерации) | Stream<String> streamFromGenerate = Stream.generate(() -> "a1") |

</details>


### У коллекции можно вызвать stream и parallelStream. Расскажите про особенности параллельного стрима?
<details><summary>Ответ</summary>

___

Кроме последовательных потоков Stream API поддерживает параллельные потоки. Распараллеливание потоков позволяет 
задействовать несколько ядер процессора (если целевая машина многоядерная) и тем самым может повысить производительность
и ускорить вычисления. В то же время говорить, что применение параллельных потоков на многоядерных машинах однозначно 
повысит производительность - не совсем корректно. В каждом конкретном случае надо проверять и тестировать.

Чтобы сделать обычный последовательный поток параллельным, надо вызвать у объекта Stream метод parallel. Кроме того, 
можно также использовать метод parallelStream() интерфейса Collection для создания параллельного потока из коллекции.

Однако не все функции можно без ущерба для точности вычисления перенести с последовательных потоков на параллельные. 
Прежде всего такие функции должны быть без сохранения состояния и ассоциативными, то есть при выполнении слева направо 
давать тот же результат, что и при выполнении справа налево, как в случае с произведением чисел. Например:

```java	
Stream<String> wordsStream = Stream.of("мама", "мыла", "раму");
String sentence = wordsStream.parallel().reduce("Результат:", (x,y)->x + " " + y);
System.out.println(sentence);
```
Результатом этой функции будет консольный вывод:

>Результат: мама Результат: мыла Результат: раму

Данный вывод не является правильным. Если же мы не уверены, что на каком-то этапе работы с параллельным потоком он 
адекватно сможет выполнить какую-нибудь операцию, то мы можем преобразовать этот поток в последовательный посредством
вызова метода *sequential()*:

```java
Stream<String> wordsStream = Stream.of("мама", "мыла", "раму", "hello world");
String sentence = wordsStream.parallel()
        .filter(s->s.length()<10) // фильтрация над параллельным потоком
        .sequential()
        .reduce("Результат:", (x,y)->x + " " + y); // операция над последовательным потоком
System.out.println(sentence);
```
И возьмем другой пример:
	
```java
Stream<Integer> numbersStream = Stream.of(1, 2, 3, 4, 5, 6);
Integer result = numbersStream.parallel().reduce(1, (x,y)->x * y);
System.out.println(result);
```
Фактически здесь происходит перемножение чисел. При этом нет разницы между 1 * 2 * 3 * 4 * (5 * 6) или 5 * 6 * 1 * 
(2 * 3) * 4. Мы можем расставить скобки любым образом, разместить последовательность чисел в любом порядке, и все равно 
мы получим один и тот же результат. То есть данная операция является ассоциативной и поэтому может быть распараллелена.

В то же время если рабочая машина не является многоядерной, то поток будет выполняться как последовательный.

*P.S. - может показаться, что ответ получился чрезмерно развернутым, но вышло совсем наоборот - чтобы описание вышло 
наиболее компактным и понятным пришлось не включать в ответ такие темы как **Вопросы производительности в 
параллельных операциях** и **Упорядоченность в параллельных потоках**. О них можно почитать отдельно.*
</details>


### Решите задачу, используя Stream.
<details><summary>Условие</summary>

___

Имеется класс User:
```java
public class User {
    String firstName;
    String lastName;
    int age;
}
```

Реализуйте метод
```java
public List<String> getUserDetail(List<User> users, int age){
    // solution
}
```
по следующим критериям:

* filter by User.age >= provided age
* map to String like firstName + lastName
* order by alphabet
* return List of String
</details>

<details><summary>Решение</summary>

___

```java
    public static List<String> getUserDetail(List<User> users, int age) {
            return users.stream()
                    .filter((User u) -> u.age >= age)
                    .map((User u) -> u.firstName + " " + u.lastName)
                    .sorted()
                    .collect(Collectors.toList());
    }
```
</details>


### Как устроена иерархия коллекций в Java? Что значит Checked и Uncheked?
<details><summary>Ответ</summary>

___

    В разработке...
</details>


### Чем отличается ArrayList от LinkedList? Сравните операции вставки в начало коллекции. Time complexity в лучшем и худшем случаях?
<details><summary>Ответ</summary>

___

    В разработке...
</details>


### Опишите, как происходит операция вставки элемента в HashMap.
<details><summary>Ответ</summary>

___

    В разработке...
</details>


### Как осуществляется разрешение коллизий в HashMap?
<details><summary>Ответ</summary>

___

    В разработке...
</details>


### Что изменилось в HashMap в Java 8?
<details><summary>Ответ</summary>

___

    В разработке...
</details>
