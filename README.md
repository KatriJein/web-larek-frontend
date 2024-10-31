# 3 курс
# Сизова Екатерина Ивановна
# Проектная работа "Веб-ларек"

Стек: HTML, SCSS, TS, Webpack

Структура проекта:
- src/ — исходные файлы проекта
- src/components/ — папка с JS компонентами
- src/components/base/ — папка с базовым кодом
- src/components/models/ — папка с кодом моделей
- src/components/views/ — папка с кодом отображения

Важные файлы:
- src/pages/index.html — HTML-файл главной страницы
- src/types/index.ts — файл с типами
- src/index.ts — точка входа приложения
- src/scss/styles.scss — корневой файл стилей
- src/utils/constants.ts — файл с константами
- src/utils/utils.ts — файл с утилитами

## Установка и запуск
Для установки и запуска проекта необходимо выполнить команды

```
npm install
npm run start
```

или

```
yarn
yarn start
```
## Сборка

```
npm run build
```

или

```
yarn build
```

## Архитектура приложения

Проект организован на основе архитектурного паттерна MVC. Контроллером выступает файл index.ts, который координирует работу модели и представлений, обрабатывая пользовательские события, изменяя состояние модели и обновляя интерфейс через представления.

**Модель (Model)** отвечает за хранение и управление данными, предоставляя интерфейсы для работы с продуктами и корзиной. Она получает данные от сервера через API, обрабатывает их, и предоставляет обновлённые данные представлениям.

**Представления (View)** отображают данные, полученные из модели, и реагируют на взаимодействие с пользователем. Представления передают события, такие как добавление товара в корзину или переход к оформлению заказа, контроллеру.

**Контроллер (Controller(index.ts)** принимает события от представлений и вызывает соответствующие методы модели для изменения данных. После обновления данных он уведомляет представления об изменениях, чтобы они отобразили актуальное состояние приложения.

### Классы

#### Базовые классы

**Api** — управляет запросами к серверу. Позволяет получать и отправлять данные для взаимодействия с API.

**EventEmitter** — управляет событиями. Обеспечивает установку и удаление слушателей, а также вызов соответствующих обработчиков при возникновении событий.
Реализует интерфейс **IEmit**:
- on<T extends object>(event: EventName, callback: (data: T) => void): void;
- emit<T extends object>(event: string, data?: T): void;
- trigger<T extends object>(event: string, context?: Partial<T>): (data: T) => void;

#### Модели 

**CartModel** — модель корзины пользователя. Обрабатывает хранение добавленных товаров, управление данными о покупателе, а также функции добавления и удаления товаров.
Реализует интерфейс **ICartModel**:
- items: Map<string, CartItem>;
- add(item: CartItem): void;    
- remove(id: string): void;

**ProductsModel** — модель для работы с каталогом товаров. Хранит данные о продуктах, а также предоставляет доступ к информации о конкретном товаре.
Реализует интерфейс **IProductsModel**:
- items: Product[];
- set(items: Product[]): void;    
- getProduct(id: string): Product;

#### Представления 

**ModalView (абстрактный)** — базовый класс для работы с модальными окнами, обеспечивает установку слушателей для событий модального окна.
Реализует интерфейс **IView**:
- render(data?: object): HTMLElement;

**CatalogView** — отображает каталог с карточками товаров. Содержит элементы типа CatalogItemView для каждого товара. Реализует интерфейс **IView**.

**CatalogItemView** — отображает отдельную карточку товара в каталоге, поддерживает интерактивные действия, такие как добавление в корзину. Реализует интерфейс **IView**

**CartModalView** (расширяет ModalView) — отображает корзину в модальном окне. Содержит элементы CartItemView для каждого товара в корзине.

**CartItemModalView** (расширяет ModalView) — отображает карточку товара в корзине внутри модального окна.

**ProductModalView** (расширяет ModalView) — отображает подробную информацию о выбранном товаре в модальном окне.

**PaymentModalView** (расширяет ModalView) — отображает окно для выбора способа оплаты и адреса.

**ContactModalView** (расширяет ModalView) — отображает окно для ввода контактных данных покупателя.

**ResultModalView** (расширяет ModalView) — отображает финальное окно с его статусом.

### Типы данных

тип **Product**:
- id: string // уникальный идентификатор продукта
- title: string // название продукта
- price: number // цена продукта
- description: string // описание продукта
- category: string // категория, к которой относится продукт
- image: string // URL-адрес изображения продукта

тип **ProductsList**:
- items: Product[] // массив объектов Product, представляющий товары
- total: number // общее количество продуктов, доступных в каталоге

тип **CartItem**:
- id: string // уникальный идентификатор продукта
- title: string // название продукта
- price: number // цена продукта на момент добавления в корзину

тип **Order**:
- payment: string // выбранный способ оплаты заказа
- email: string // email покупателя
- address: string // адрес доставки
- phone: string // контактный телефон покупателя
- total: number // общая сумма заказа
-items: string[] // массив идентификаторов товаров, включённых в заказ

тип **OrderResp**:
- id: string // уникальный идентификатор созданного заказа
- total: number // общая сумма подтверждённого заказа
