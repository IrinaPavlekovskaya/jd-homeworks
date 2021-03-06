# Задача Перепись населения

## Описание
Представьте, что Вас попросили помочь с анализом данных по переписи населения какого-либо крупного города. Это может потребовать значильной вычислительной мощности и времени на решение. В данной задаче, мы посмотрим, как можно работать с стримами, используя параллельные вычисления.

Мы обратимся к опыту предыдущей задачи, немного модифицировав стартовые условия. Например, добавим в класс `Sex` статический метод `randomSex()`, который позволит получить случайный пол:
```java
public enum Sex {
    MAN,
    WOMEN;

    private static final List<Sex> VALUES = List.of(values());
    private static final int SIZE = VALUES.size();
    private static final Random RANDOM = new Random();

    public static Sex randomSex()  {
        return VALUES.get(RANDOM.nextInt(SIZE));
    }
}
```
Нам потребуется действительно большое количство данных. Для примера, будем считать, что мы производим перепись населения города Сеул с населением в 10 миллионов человек. Для их генерации воспользуемся следующим способом:
```java
List<String> names = Arrays.asList("Ким", "Ли", "Пак", "Чен", "Цой");
List<People> peoples = new ArrayList<>();
for (int i = 0; i < 10_000_000; i++) {
    peoples.add(new People(names.get(
            new Random().nextInt(names.size())),
            new Random().nextInt(100),
            Sex.randomSex()));
}
```
Так как Вы будем использовать параллельные вычисления, то интересно посмотреть, действительно ли они так полезны. Добавьте несколько строчек, которые позволят засечь время выполнения операций с данными. Данну строчку добавим сразу после генерации `peoples`:
```java
long startTime = System.nanoTime(); 
```
А эти в самом конце метода `main()`:
```java
long stopTime = System.nanoTime();
double processTime = (double) (stopTime - startTime) / 1_000_000_000.0;
System.out.println("Process time: " + processTime + " s");
```
Задание, в целом, остается прежним. Его нужно выполнить дважды для обычного расчета и с использованием параллельных вычислений. Из коллеции объектов `People` необходимо:
1. Выбрать мужчин-военнообязанных и вывести их количество в консоль.
2. Найти средний возраст среди мужчин и вывести его в консоль.
3. Найти кол-во потенциально работоспособных людей в выборке (т.е. от 18 лет и учитывая что женщины выходят в 55 лет, а мужчина в 60).

## Реализация
Перерабойте получение списка мужчин-военнообязанных в их колличество. Выводите в консоль только их число!

Убедившись в том, что Вы выводите в консоль не список военно-обязанных мужчин, а лишь их количество, запустите расчет.

С помощью комментария укажите время выполнения программы. Если Ваш компьютер не справляется с поставленной задачей и начинает подвисать, то уменьшайте количество данных до приемлимого значения.

Далее, примените параллельные вычисления. Для этого замените получение стрима из коллекции с метода `stream()` на `parallelStream()` для всех этапов анализа данных. После этого запустите программу еще раз. Так же с помощью комментария укажите время выполнения.

## Дополнительное задание(*)
Интересно разобраться, с какого момента параллельные вычисления начинают приносить пользу. Произведите вышеизложенную операцию для выборки из 100, 1_000, 10_000, 100_000, 1_000_000 и 10_000_000 объектов. Проанализируйте время выполнения и посчитайте, на сколько процентов уменьшается время для каждой из выборок. Если Вам известна модель Вашего процессора, то укажите ее. 
