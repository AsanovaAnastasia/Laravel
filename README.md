## 14.  Создание администраторской панели

Цели практической работы:

- Научиться интегрировать админ-панель в проект.
- Разобраться в настройке CRUD-методов для сущности в voyager.

Что нужно сделать:

В этой практической работе вы создадите панель администратора для склада интернет-магазина.

1. Создайте новый проект Laravel или откройте уже существующий.

2. Создайте новую ветку вашего репозитория от корневой (main или master).

3. Создайте класс Category (модель, миграцию и контроллер) командой `php artisan make:model Category -m`

4. Опишите миграцию для таблицы categories c типами полей:

```
$table->id();
$table->string('name');
$table->timestamps();
```

5. Создайте класс Product (модель, миграцию и контроллер) командой `php artisan make:model Product -m`

6. Опишите миграцию для таблицы products c типами полей:

```
$table->string('sku');
$table->id();
$table->string('sku');
$table->string('name');
$table->foreignId('category_id')->constrained();
```

7. Выполните миграцию командой `php artisan migrate`

8. Установите voyager командой `composer require tcg/voyager`

9. Выполните установку voyager внутри вашего приложения командой `php artisan voyager:install`

10. Создайте администратора вашего приложения командой `php artisan voyager:admin your@email.com`

11. Войдите в панель администратора, перейдите во вкладку tools/bread и добавьте возможность редактирования сущностей category и product.

12. После создания CRUD для сущности product перейдите в эту сущность и нажмите на кнопку Create A Relationship.

13. Настройте связь следующим образом:

![](sem13.png) 

14. Сохраните связь.

15. Создайте категорию, а после — тестовый товар, прикреплённый к этой категории.

16. Создайте в проекте директорию App/Admin/Widgets и добавьте туда два виджета: ProductsWidget и CategoriesWidget.

17. Реализуйте в этих виджетах счётчики количества товаров и категорий.

18. Добавьте виджеты в конфигурационный файл voyager.php:

```
'widgets' => [
\App\Admin\Widgets\ProductsWidget::class,
\App\Admin\Widgets\CategoriesWidget::class,
],
```

Скрины к  ДЗ

![](scrins/Screenshot_1.jpg) 
![](scrins/Screenshot_2.jpg) 
![](scrins/Screenshot_3.jpg)
![](scrins/Screenshot_4.jpg) 
![](scrins/Screenshot_5.jpg)
![](scrins/Screenshot_6.jpg)
![](scrins/Screenshot_7.jpg)    

## 13. Тестирование и отладка Laravel-приложений

Научиться:

- создавать класс-фабрику и класс-наполнитель и использовать их;
-  создавать контроллер и тестировать его с помощью Postman;
-  писать feature-тесты для проверки работы методов контроллера.

Что нужно сделать:

В этой практической работе вы реализуете уведомления через внешние сервисы.

1. Создайте новый проект Laravel или откройте уже существующий.

2. Создайте новую ветку вашего репозитория от корневой (main или master).

3. Создайте сущность Product (модель, миграцию и контроллер) командой `php artisan make:model Product -mc`.

4. Опишите миграцию для таблицы products c типами полей:
```
$table->string('sku');
$table->string('name');
$table->decimal('price', 9, 3);
```

5. Выполните миграцию командой `php artisan migrate`.


6. Добавьте в файл api.php маршруты:

   `Route::apiResource('products', \App\Http\Controllers\ProductController::class);`

7. Создайте класс-фабрику для сущности Product c помощью команды `php artisan make:factory ProductFactory`.

8. Создайте класс-наполнитель для сущности Product c помощью команды `php artisan make:seeder ProductsSeeder`.

9. Выполните команду `php artisan migrate –-seed` для наполнения базы данных сгенерированными данными.

10. В классе ProductController реализуйте методы index, show, store, update, destroy.

11. Протестируйте каждый из маршрутов контроллера ProductController с помощью Postman и приложите скриншоты ответа на запросы в папку postman-screenshots (названия файлов должны соответствовать формату index.jpeg, show.jpeg, store.jpeg, update.jpeg, destroy.jpeg для каждого метода, соответственно).

![](postman-src/index.png)

![](postman-src/destroy.png)

![](postman-src/show.png)

![](postman-src/store.png) 

![](postman-src/update.png)

12. Создайте тест c помощью команды `php artisan make:test Products/ProductTest`

13. Опишите функции:
```
test_products_can_be_indexed,
test_product_can_be_shown,
test_product_can_be_stored,
test_product_can_be_updated,
test_product_can_be_destroyed.
```
14. Запустите выполнение тестов командой `php artisan test`.

## 12. Интеграция с внешними сервисами

Научиться:
- интегрировать отправку писем через почтовый клиент;
- настраивать отправку сообщений в мессенджер.

Что нужно сделать:

В этой практической работе вы реализуете уведомления через внешние сервисы.

1. Создайте новый проект Laravel или откройте уже существующий.
2. Создайте новую ветку вашего репозитория от корневой (main или master).
3. Настройте регистрацию и аутентификацию пользователей.
4. Настройте почтовый клиент любого сервиса.
5. Впишите в файл .env нужные значения для почтового сервиса.
6. Создайте письмо Welcome.php командой `php artisan make:mail Welcome`.
7. В конструкторе класса присвойте свойству класса $user параметр конструктора класса.

```
public User $user;
public function __construct(User $user)
{
$this->user = $user;
}
```

8. Создайте шаблон мейлинга welcome.blade.php в директории resources/views/emails с кодом внутри

`Добрый день, {{ $user->name }}, спасибо за регистрацию.`

9. Добавьте код отправки вашего письма в функцию store класса `RegisteredUserController.`
10. Подключите клиент мессенджера Telegram командой `composer require irazasyed/telegram-bot-sdk`
11. Создайте бота и канал, добавьте бота в телеграм-канал.
12. Укажите в файле .env значения, необходимые для работы бота.
13. Проверьте работу бота с помощью тестового маршрута.

```
Route::get('test-telegram', function () {
Telegram::sendMessage([
'chat_id' => env('TELEGRAM_CHANNEL_ID', ''),
'parse_mode' => 'html',
'text' => 'Произошло тестовое событие'
]);
return response()->json([
'status' => 'success'
]);
});
```

14. Добавьте код уведомления в мессенджер о новом пользователе вашей системы в функцию store класса RegisteredUserController.
15. Зарегистрируйтесь на сайте.
16. Проверьте, что сообщение отправлено на электронную почту (рекомендуется использовать для регистрации тот почтовый ящик, с которого отправляется сообщение, чтобы избежать блокировки адреса за спам).
17. Проверьте, что в Telegram пришло уведомление о регистрации нового пользователя.


