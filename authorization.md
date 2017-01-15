git 1be1791e0168323f0e23b50fd7b64794e53709c6

---

# Авторизация

- [Введение](#introduction)
- [Шлюзы](#gates)
    - [Написание шлюзов](#writing-gates)
    - [Авторизация действий](#authorizing-actions-via-gates)
- [Создание политик](#creating-policies)
    - [Генерация политик](#generating-policies)
    - [Регистрация политик](#registering-policies)
- [Написание Политик](#writing-policies)
    - [Методы политик](#policy-methods)
    - [Методы, которые не требуют модели](#methods-without-models)
    - [Фильтры политик](#policy-filters)
- [Авторизация действий с помощью политик](#authorizing-actions-using-policies)
    - [Используя модель пользователя](#via-the-user-model)
    - [Используя посредник](#via-middleware)
    - [Используя хелперы контроллера](#via-controller-helpers)
    - [Используя шаблоны Blade](#via-blade-templates)

<a name="introduction"></a>
## Введение

В дополнение к предоставленным из коробки службам [аутентификации](/docs/{{version}}/authentication) , Laravel также предоставляет простой способ авторизовать действия пользователя в отношении данного ресурса. Как и в случае с аутентификацией, подход Laravel к авторизации прост, и есть два основных способа авторизации действий: "шлюзы"(Gates) и "политики"(Policies).

Думайте о шлюзах и политиках, как маршрутах и контроллерах. Шлюзы обеспечивают простое решение на основе замыканий, в свою очередь политики, как контроллеры, группируют свою логику вокруг конкретной модели или ресурса. Сначала мы разберем шлюзы, а затем изучим политики.

Важно не рассматривать шлюзы и политики как взаимоисключающие вещи. Большинство приложений, скорее всего, содержат смесь шлюзов и политик, и это прекрасно! Шлюзы наиболее применимы к действиям, которые не связаны с какой-либо моделью или ресурсом, например, просмотр панели администратора. В противоположность этому, политика должна быть использована, если вы хотите разрешить действие для конкретной модели или ресурса.

<a name="gates"></a>
## Шлюзы

<a name="writing-gates"></a>
### Написание шлюзов

Шлюзы - это замыкания, которые определяют, имеет ли пользователь право выполнить данное действие. И обычно, определяются в классе `App\Providers\AuthServiceProvider` с помощью фасада `Gate`. Шлюз всегда получает экземпляр пользователя в качестве первого аргумента. Так же они могут принимать дополнительные аргументы, например, соотвтетствующую модель Eloquent:

```php
/**
 * Register any authentication / authorization services.
 *
 * @return void
 */
public function boot()
{
    $this->registerPolicies();

    Gate::define('update-post', function ($user, $post) {
        return $user->id == $post->user_id;
    });
}
```

<a name="authorizing-actions-via-gates"></a>
### Авторизация действий

Чтобы авторизовать действие с помощью шлюза, вы должны использовать метод `allows`. Обратите внимание, что вам не нужно передавать текущего, аутентификацированного пользователя в метод `allows`. Laravel автоматически подставит текущего пользователя в замыкание:

```php
if (Gate::allows('update-post', $post)) {
    // Текущий пользователь может обновить пост...
}
```

Если вы хотите определить, авторизован ли конкретный пользователь для выполнения действия, вы можете использовать метод `forUser` фасада ` Gate`:

```php
if (Gate::forUser($user)->allows('update-post', $post)) {
    // Пользователь может обновить пост...
}
```

<a name="creating-policies"></a>
## Создание политик

<a name="generating-policies"></a>
### Генерация политик

Политики являются классами, организующими логику авторизации вокруг конкретной модели или ресурса. Например, если ваше приложение является блогом, вы можете иметь модель `Post` и соответствующую `PostPolicy` для  авторизации действия пользователя, таких как создание или обновление постов.

Вы можете создать политику, используя  [команду artisan](/docs/{{version}}/artisan) `make:policy`. Сформированная политика будет помещена в каталог `app/Policies`. Если этот каталог не существует в приложении, Laravel создаст его:

```
php artisan make:policy PostPolicy
```


Команда `make:policy` создаст пустой класс политики. Если вы хотите создать класс c базовыми "CRUD" методами уже включенными в политику, вы можете указать опцию `--model` при выполнении команды:

```
php artisan make:policy PostPolicy --model=Post
```

> Все политики создаются через Laravel [сервис-контейнер](/docs/{{version}}/container), это позволяет внедрять любые необходимые зависимости в конструктор этой политики, эти зависимости будут разрешены автоматически.

<a name="registering-policies"></a>
### Регистрация политик

После того, как политика была создана, она должна быть зарегистрирована. `AuthServiceProvider` входящий в состав свежеустановленного приложения Laravel содержит свойство `policies`, которое сопоставляет ваши Eloquent модели соответствующим политикам. Регистрация политики будет указывать Laravel, какую политику использовать при авторизации действия для данной модели:

    <?php

    namespace App\Providers;

    use App\Post;
    use App\Policies\PostPolicy;
    use Illuminate\Support\Facades\Gate;
    use Illuminate\Foundation\Support\Providers\AuthServiceProvider as ServiceProvider;

    class AuthServiceProvider extends ServiceProvider
    {
        /**
         * The policy mappings for the application.
         *
         * @var array
         */
        protected $policies = [
            Post::class => PostPolicy::class,
        ];

        /**
         * Register any application authentication / authorization services.
         *
         * @return void
         */
        public function boot()
        {
            $this->registerPolicies();

            //
        }
    }

<a name="writing-policies"></a>
## Написание политик

<a name="policy-methods"></a>
### Методы политик


После того, как политика была зарегистрирована, вы можете добавить методы для всех действий, которые она авторизует. Например, давайте определим метод `update`  нашего класса `PostPolicy`, который определяет, может ли данный пользователь `User` обновить данный экземпляр `Post`.

Метод `update` получит `User`'a и экземпляр `Post` в качестве аргументов, и он должен вернуть `true` или `false`, указывающее имеет ли пользователь право обновлять данный `Post`. В данном примере, давайте проверим, что `id` пользователя соответствует `user_id` на поста:

```php
<?php

namespace App\Policies;

use App\User;
use App\Post;

class PostPolicy
{
    /**
        * Determine if the given post can be updated by the user.
        *
        * @param  \App\User  $user
        * @param  \App\Post  $post
        * @return bool
        */
    public function update(User $user, Post $post)
    {
        return $user->id === $post->user_id;
    }
}
```

Вы можете ghjl определять дополнительные методы по мере необходимости политики для различных действий, которые он санкционирует. Например, вы можете определить методы `view`(просмотр) или  `delete`(удаление) для авторизации различных действий над моделью `Post`. И не забывайте, что вольны называть методы ваших политик как вам хочется. 

> Если вы использовали опцию `--model`  при создании вашей политики из консоли Artisan, она уже будет включать методы для действий `view`, `create`,` update` и `delete`.

<a name="methods-without-models"></a>
### Методы без модели


Некоторые методы политики получиают только текущего аутентифицированного пользователя но не экземпляр модели, действие над которой они авторизуют. Такая ситуация чаще всего встерчается при авторизации действия `create`. Например, если вы создаете блог, вы можете проверить, имеет ли пользователь право создавать какие либо посты.

При определении метода политики, которая не получает экземпляр модели, например метод `create` вы должны определить метод, который ожидет на вход только аутентифицированного пользователя:

```php
/**
    * Determine if the given user can create posts.
    *
    * @param  \App\User  $user
    * @return bool
    */
public function create(User $user)
{
    //
}
```

> Если вы использовали опцию `--model` при создании вашей политики, все соответствующие политики "CRUD"-методы уже будут определены в сгенерированной политике.

<a name="policy-filters"></a>
### Фильтры политик

Для некоторых пользователей, вы можете разрешить все действия в рамках данной политики. Для достижения этой цели, определите  метод `before`. Метод `before` будет выполняться перед любыми другими методами политики, это даст вам возможность авторизовать действия до того, как конкретный метод политики будет выполнен. Эта функция наиболее часто используется для авторизации выполнения каких-либо действий администратором приложения:

```php
public function before($user, $ability)
{
    if ($user->isSuperAdmin()) {
        return true;
    }
}
```
<a name="authorizing-actions-using-policies"></a>
## Авторизация действий с помощью политик

<a name="via-the-user-model"></a>
### Используя модель пользователя

Модель `User`, которая поставляется с Laravel включает в себя две полезных метода для авторизации действий: `can` и `cant`. Метод `can` принимает действие, которое вы хотите разрешить и соответствующую модель. Например, давайте определим, имеет ли пользователь право обновлять данную модель `Post`:

```php

    if ($user->can('update', $post)) {
        //
    }
```

Если  для данной модели [зарегистрирована политика](#registering-policies) , метод `can` будет автоматически вызывать соответствующую политику и вернет булево значение. Если для данной модели политика не зарегистрирована метод  `can` попытается вызвать замыкание шлюза, соответствующее данному названию действия.

#### Действия которые не требуют модели

Как вы помните, некоторые действия, такие как `create` не могут требовать экземпляр модели. В таких ситуациях, вы можете передать в метод `can` имя класса. Имя класса будет использоваться для определения того, какие политики использовать при авторизации действия:

```php
use App\Post;

if ($user->can('create', Post::class)) {
    // Executes the "create" method on the relevant policy...
}
```

<a name="via-middleware"></a>
### Используя посредники

Laravel включает в себя посредники, которые могут авторизовать действия до того, как входящий запрос достигнет маршрутов или контроллеров. По умолчанию,  в `App\Http\Kernel` посреднику `Illuminate\Auth\Middleware\Authorize` назначен ключ `can`. Давайте рассмотрим пример использования посредника `can` для авторизации пользователя может на обновление поста в блоге:

```php
use App\Post;

Route::put('/post/{post}', function (Post $post) {
    // The current user may update the post...
})->middleware('can:update,post');
```

В этом примере мы передаем в  посредник `can` два аргумента. Первый - это имя действия, которое мы хотим разрешить, а второй - имя параметра маршрута, который мы хотим передать методу политики. В этом случае, так как мы используем [неявную привязку модели](/docs/{{version}}/routing#implicit-binding), модель `Post` будет передана методу политики. Если пользователь не имеет права выполнить данное действие, посредником будет сгенерирован HTTP-ответ с кодом `403`.


#### Действия, которые не требуют модели

Опять же, некоторые действия, такие как `create` не могут требовать экземпляр модели. В таких ситуациях, вы можете передать в посредник имя класса. Имя класса будет использоваться для определения того, какие политики будут использоваться при авторизации действия:

```
Route::post('/post', function () {
    // Текущий пользователь может создать пост...
})->middleware('can:create,App\Post');
```
<a name="via-controller-helpers"></a>
### Используя хелперы контроллеров


В дополнение к полезным методам, предусмотренным в модели `User`, Laravel предоставляет еще один полезный метод `authorize`  в любом из контроллеров, которые расширяют базовый класс `App\Http\Controllers\Controller`. Как и метод  `can`, этот метод принимает имя действия вы хотите разрешить и соответствующую модель. Если действие не разрешено, то `authorize` сгенерирует исключение `Illuminate\Auth\Access\AuthorizationException`, которое обработчик исключений Laravel преобразует в ответ HTTP с статус кодом `403`:

```php
<?php

namespace App\Http\Controllers;

use App\Post;
use Illuminate\Http\Request;
use App\Http\Controllers\Controller;

class PostController extends Controller
{
    /**
        * Update the given blog post.
        *
        * @param  Request  $request
        * @param  Post  $post
        * @return Response
        */
    public function update(Request $request, Post $post)
    {
        $this->authorize('update', $post);

        // The current user can update the blog post...
    }
}
```
#### Действия, которые не требуют модели

Как обсуждалось ранее, некоторые действия, например `create` не могут требовать экземпляр модели. В таких случаях, вы можете передать имя класса в метод `authorize`. Имя класса будет использоваться для определения того, какие политики будут использоваться при авторизации действия:

```
/**
    * Create a new blog post.
    *
    * @param  Request  $request
    * @return Response
    */
public function create(Request $request)
{
    $this->authorize('create', Post::class);

    // The current user can create blog posts...
}
```

<a name="via-blade-templates"></a>
### Используя шаблоны Blade

При написании шаблонов Blade, вам может понадобиться отобразить часть страницы, только если пользователь авторизован выполнить данное действие. Например, вы можете показать форму обновления для в блоге, только если пользователь действительно может обновить пост. В этом случае, вы можете использовать директивы  `@can` и `@cannot`.

```blade
    @can('update', $post)
        <!-- Текущий пользователь может обновить пост -->
    @endcan

    @cannot('update', $post)
        <!-- Текущий пользователь не может обновить пост  -->
    @endcannot
```

Эти директивы являются удобными краткими записями `@if` и `@unless`. Директивы `@can` и `@cannot`  указанные выше можно разложить как:

```blade
    @if (Auth::user()->can('update', $post))
        <!-- Текущий пользователь может обновить пост -->
    @endif

    @unless (Auth::user()->can('update', $post))
        <!-- Текущий пользователь не может обновить пост  -->
    @endunless
```

#### Действия, которые не требуют модели

Как и большинство других методов авторизации, вы можете передать имя класса в `@ законсервирует` и `@ cannot` директивы, если действие не требует экземпляра модели:

```blade
    @can('create', Post::class)
         <!-- Текущий пользователь может создать пост -->
    @endcan

    @cannot('create', Post::class)
         <!-- Текущий пользователь не может создать пост -->
    @endcannot
```