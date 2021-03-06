## Глава восьма

> Умовні оператори…

Комп’ютерні програми, як і саме життя, сповнені складних рішень, які потрібно приймати. Такі речі, як: _якщо_ я лежатиму більше у ліжку, я довше спатиму, _інакше_ мені потрібно буде йти на роботу; _якщо_ я піду на роботу, я зароблю гроші, _інакше_ я втрачу свою роботу - і так далі…

Ми вже виконували декілька `if`–перевірок у попередніх програмах. Ось простий приклад з обчислювача податку з першої глави:

```ruby
if (subtotal < 0.0) then
  subtotal = 0.0
end
```

Ця програма просила користувача ввести значення `subtotal`, яке використовувалось для обчислення податку. Маленька перевірка вище страхує нас від того, що `subtotal` буде від’ємним. Якщо користувач, в божевільному пориві введе значення, яке менше за `0`, перевірка `if` це помітить, оскільки (`subtotal < 0.0`) буде істинним, а це спричинить до того, що код між перевіркою `if` та ключовим словом `end` виконається. У нашому випадку він встановить значення `subtotal` рівне `0`.

> ### Одне дорівнює `=` чи два `==`?
>
> Як і у більшості мовах програмування, Ruby використовує один знак рівності для присвоєння значення `=`, а два — для порівняння значень `==`.

## `if..then..else`

[**`if_else.rb`**](https://github.com/LambdaBooks/thelittlebookofruby/blob/master/examples/8/if_else.rb):

Проста перевірка `if` має лише два можливих результати. Код або виконається, або ні, в залежності від істинності або хибності перевірки.

Часто вам потрібно мати більше, ніж два можливих результати. Давайте припустимо, наприклад, що програма повинна виконуватись одним способом, якщо день тижня будній та іншим — якщо вихідним. Ви можете перевірити це, додавши секцію `else` після секції `if`, ось так:

```ruby
if aDay == 'Субота' or aDay == 'Неділя'
  daytype = 'вихідний'
else
  daytype = 'будень'
end
```

Тут умова `if` є простою. Вона перевіряє дві можливі умови:

1. значення змінної `aDay` рівне рядку `'Субота'` або…
2. значення змінної `aDay` рівне рядку `'неділя'`.

Якщо якась з цих умов є істинною, тоді виконується наступний рядок:

```ruby
daytype = 'вихідний'
```

У всіх інших випадках виконується код після `else`:

```ruby
daytype = 'будень'
```

[**`if_then.rb`**](https://github.com/LambdaBooks/thelittlebookofruby/blob/master/examples/8/if_then.rb):

> Коли перевірка `if` та код, який треба виконати, знаходяться на різних рядках, ключове слово `then` є необов’язковим. Коли перевірка і код розміщені на одному рядку, між ними обов’язково має стояти ключове слово `then` (або, якщо ви надаєте перевагу лаконічному коду, двокрапка):
>
> ```ruby
> if x == 1 then puts('ok') end    # з 'then'
> if x == 1 : puts('ok') end       # з двокрапкою
> if x == 1 puts('ok') end         # синтаксична помилка!
> ```

Перевірка `if` не обмежена виконанням лише двох умов. Припустимо, наприклад, що ваш код має розуміти, чи певний день є робочим днем чи святковим. Всі будні є робочими днями; всі суботи є святковими днями, але неділі є лише святами, коли ви не працюєте понаднормово.

> **Примітка перекладача:**
>
> У прикладі вище наведений переклад оригінального тексту. Автор книги — американець, а в Сполучених Штатах Америки неділя вважається першим днем тижня.

Це моя перша спроба написати перевірку, яка задовольнятиме цим умовам:

```ruby
working_overtime = true

if aDay == 'Субота' or aDay == 'Наділя' and not working_overtime
  daytype = 'свято'
  puts( "Уурраа!" )
else
  daytype = 'робочий день'
end
```

[**`and_or.rb`**](https://github.com/LambdaBooks/thelittlebookofruby/blob/master/examples/8/and_or.rb):

На жаль, це не працює достатньо добре. Пам’ятайте, що субота є завжди святом. Проте цей код наполягає, що `'субота'` — робочий день. Це стається тому, що Ruby сприймає перевірку так: _“Якщо цей день — субота і я не працюю понаднормово, або якщо цей день — неділя, і я не працюю понаднормово”_ — проте насправді я мав на увазі: _“Якщо це день — субота або якщо цей день — неділя, і я не працюю понаднормово”_.

Найпростіший спосіб вирішити цю неточність — огорнути перевірки, які мають трактуватись як одне ціле, в круглі дужки, ось так:

```ruby
if aDay == 'Субота' or (aDay == 'Неділя' and not working_overtime)
```

## `and`, `or`, `not`

Між іншим, Ruby має два різні записи для булевих умов.

У прикладі, наведеному вище, я використав оператори у вигляді англійських слів: `and` (і/та), `or` (або) та `not` (ні). Якщо хочете, ви можете використовувати альтернативні оператори, подібні до тих, які є у інших мовах програмування, а саме: `&&` (і/та), `||` (або) та `!` (ні).

Тим не менше, будьте обережні, ці дві групи операторів не повністю взаємозамінні. Зокрема, вони мають різну пріоритетність, а це означає, що при використанні різних операторів у перевірці, її частини можуть виконуватись по–різному в залежності від того, який оператор ви використовуєте.

## `if..elsif`

Без сумнівно бувають ситуації, коли вам необхідно виконати різні дії для різних альтернативних умов. Один зі способів зробити це — виконання умови `if`, після якої слідуватиме серія інших перевірок з ключовими словами `elsif`. Після всіх перевірок мусить стояти ключове слово `end`.

[**`if_elsif.rb`**](https://github.com/LambdaBooks/thelittlebookofruby/blob/master/examples/8/if_elsif.rb):

Наприклад, тут я почергово, в циклі `while`, прошу користувача ввести дані. Умова `if` перевіряє, чи користувач ввів `'q'` (я використовую метод `chomp()`, щоб видалити небажані символи з вводу). Якщо `'q'` не введено, перша умова `elsif` перевіряє, чи цілочисельне значення введених даних (`input.to_i`) більше за 800. Якщо результат перевірки є хибним наступна умова `elsif` перевіряє, чи цілочисельне значення є меншим або більшим за 800:

```ruby
while input != 'q' do
  puts("Введіть число від 1 до 1000 (або 'q' для виходу)")
  print("?- ")
  input = gets().chomp()
  if input == 'q'
    puts("Бувай")
  elsif input.to_i > 800
    puts("Це зависокий рівень оплати!")
  elsif input.to_i <= 800
    puts("Це ми потягнемо")
  end
end
```

> Цей код має баг. Він просить ввести число від 1 до 1000, проте приймає й інші числа. Давайте спробуємо переписати цю перевірку без помилок!

[**`if_else_alt.rb`**](https://github.com/LambdaBooks/thelittlebookofruby/blob/master/examples/8/if_else_alt.rb):

> Ruby також має скорочену форму запису `if..then..else` в якій знак питання `?` заміняє `if..then`, а двокрапка поводиться як `else`…
>
> ```
> <умова> ? <якщо істина> : <якщо хиба>
> ```
>
> Наприклад:
>
> ```ruby
> x == 10 ? puts("це 10") : puts("це якесь інше число")
> ```
>
> Коли перевірка умови є комплексною (якщо вона використовує `and` та `or`), вам слід огорнути її у дужки.
>
> Якщо перевірка та код займають декілька рядків, `?` має стояти на тому ж рядку, що й умова, що передує йому, а `:` повинна бути на тому ж рядку, що і код, який йде після `?`.
>
> Іншими словами, якщо ви розмістити перехід на новий рядок перед `?` або `:`, ви отримаєте синтаксичну помилку. Ось приклад правильного багаторядкового блоку коду:
>
> ```ruby
> (aDay == 'Субота' or aDay == 'Неділя') ?
>   daytype = 'вихідний' :
>   daytype = 'будень'
> ```

## `unless`

[**`unless.rb`**](https://github.com/LambdaBooks/thelittlebookofruby/blob/master/examples/8/unless.rb):

Ruby також може виконувати перевірку `unless`, яка є протилежною до перевірки `if`:

```ruby
unless aDay == 'Субота' or aDay == 'Неділя'
  daytype = 'вихідний'
else
  daytype = 'будень'
end
```

Думайте про `unless` як про альтернативний спосіб перевірити _якщо не_. Ось відповідний еквівалент коду наведеного вище:

```ruby
if !(aDay == 'Субота' or aDay == 'Неділя')
  daytype = 'вихідний'
else
  daytype = 'будень'
end
```

## Модифікатори `if` та `unless`

Ви напевно пригадуєте альтернативний запис для циклу `while` з Глави 7. Замість того, щоб писати так…

```ruby
while tired do sleep end    # поки втомлений спати
```

…ми можемо переписати це ось так:

```ruby
sleep while tired    # спати поки втомлений
```

Альтернативний запис, при якому ключове слово `while` розташоване між кодом, який треба виконати та тестовою умовою, називається _модифікатор `while`_. Так от, Ruby також має модифікатори `if` та `unless`. Ось кілька прикладів:

[**`if_unless_mod.rb`**](https://github.com/LambdaBooks/thelittlebookofruby/blob/master/examples/8/if_unless_mod.rb):

```ruby
sleep if tired  # спати якщо втомлений

begin           #
  sleep         #   спати
  snore         #   хропіти
end if tired    # якщо втомлений

sleep unless not tired  # не спати якщо не втомлений

begin                 #
  sleep               #   спати
  snore               #   хропіти
end unless not tired  # якщо не не втомлений
```

Стислість такого запису корисна, коли, приміром, вам потрібно швидко виконати добре визначену дію, якщо умова є істинною.

Ось, як ви можете додати у ваш код зневаджувальний вивід, якщо константа `DEBUG` має істинне значення:

```ruby
puts("somevar = #{somevar}") if DEBUG
```

## Оператор `case`

Коли вам потрібно виконати ряд різних дій, в залежності від значення однієї змінної, декілька перевірок `if..elsif` є занадто багатослівними і містять багато повторень. Елегантне альтернативне рішення пропонує оператор `case`. Він починається зі слова `case`, після якого йде назва змінної для перевірки. Після цього йде ряд секцій `when`, кожна з яких визначає _пускове_ значення для коду, який йде після неї. Цей код виконується, лише коли значення змінної, яка перевіряється, дорівнює пусковому значенню:

[**`case.rb`**](https://github.com/LambdaBooks/thelittlebookofruby/blob/master/examples/8/case.rb):

```ruby
case(i)
  when 1 : puts("Понеділок")
  when 2 : puts("Вівторок")
  when 3 : puts("Середа")
  when 4 : puts("Четвер")
  when 5 : puts("П’ятниця")
  when (6..7) : puts("Юху! Вихідні!")
  else puts("Такого дня не існує!")
end
```

У цьому прикладі я використовую двокрапку, щоб відділити перевірку `when` від коду, який має виконуватись. Ви також можете використовувати ключове слово `then`:

```ruby
when 1 then puts("Понеділок")
```

Двокрапка або `then` можна упустити, якщо перевірка та код, який виконається, розміщені на різних рядках. На відміну від оператора `case` у C-подібних мовах, немає потреби вставляти ключове слово `break` для того, що перервати перехід виконання до секцій, які знаходяться нижче.

В Ruby, як тільки знайдене співпадіння, оператор `case` завершує роботу:

```ruby
case(i)
  when 5 : puts("П’ятниця")
    puts("...майже вихідні!")
  when 6 : puts("Субота!")
  # the following never executes
  when 5 : puts("А тепер знову п’ятниця!")
end
```

Ви можете декілька рядків коду між кожною умовою `when`, і ви можете перелічити декілька значень розділених комами, щоб запустити певний блок `when`, ось так:

```ruby
when 6, 7 : puts("Юху! Вихідні!")
```

Умова в операторі `case` не обов’язково має бути простою змінною, це може бути і вираз, як от:

```ruby
case(i + 1)
```

Ви також можете використовувати нецілочисельні типи, як от рядки.

Декілька пускових значень визначених у секції `when` можуть мати різні типи – наприклад, і рядки, і цілі числа:

```ruby
when 1, 'Monday', 'Mon' : puts("Так, '#{i}' це понеділок")
```

[**`case2.rb`**](https://github.com/LambdaBooks/thelittlebookofruby/blob/master/examples/8/case2.rb):

Ось більш розгорнутий приклад, який ілюструє синтаксичні елементи, про які йшлось раніше:

```ruby
case(i)
  when 1 : puts("Понеділок")
  when 2 : puts("Вівторок")
  when 3 : puts("Середа")
  when 4 : puts("Четвер")
  when 5 then puts("П’ятниця")
    puts("...майже вихідні!")
  when 6, 7
    puts("Субота!") if i == 6
    puts("Неділя!") if i == 7
    puts("Юху! Вихідні!")
    # все, що нижче ніколи не виконається
  when 5 : puts("А тепер знову п’ятниця!")
  else puts("Такого дня не існує!")
end
```
