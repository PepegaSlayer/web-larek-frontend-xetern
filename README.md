# 3 Курс

# Рябков Георгий Константинович

# Проектная работа "Веб-ларек"

Стек: HTML, SCSS, TS, Webpack

Структура проекта:
- src/ — исходные файлы проекта
- src/components/ — папка с JS компонентами
- src/components/base/ — папка с базовым кодом

Важные файлы:
- src/pages/index.html — HTML-файл главной страницы
- src/types/index.ts — файл с типами
- src/index.ts — точка входа приложения
- src/scss/styles.scss — корневой файл стилей
- src/utils/constants.ts — файл с константами
- src/utils/utils.ts — файл с утилитами

## Структура проекта

### 1. Типы (types)

В папке `src/types` определены интерфейсы, которые описывают структуру данных, используемых в приложении:

- **IProductModel**: Описывает модель продукта.
  - `id: string`: Уникальный идентификатор продукта.
  - `title: string`: Название продукта.
  - `description: string`: Описание продукта.
  - `price: number`: Цена продукта.
  - `image: string`: URL изображения продукта.
  - `category: string`: Категория продукта.

- **ICartModel**: Описывает модель корзины.
  - `products: IProductModel[]`: Массив продуктов в корзине.
  - `add(product: IProductModel): void`: Метод для добавления продукта в корзину.
  - `remove(id: string): void`: Метод для удаления продукта из корзины.

- **ICatalogModel**: Описывает модель каталога.
  - `products: IProductModel[]`: Массив продуктов в каталоге.
  - `setItems(items: IProductModel[]): void`: Метод для установки продуктов в каталоге.
  - `getProduct(id: string): IProductModel`: Метод для получения продукта по его идентификатору.

- **IEventEmitter**: Описывает интерфейс для работы с событиями.
  - `emit(event: string, data?: unknown): void`: Метод для эмитации события.

- **IOrder**: Описывает модель заказа.
  - `id: string`: Уникальный идентификатор заказа.
  - `payment: string`: Способ оплаты.
  - `mail: string`: Электронная почта клиента.
  - `phone: string`: Телефон клиента.
  - `address: string`: Адрес доставки.
  - `total: number | null`: Общая сумма заказа.
  - `products: IProductModel[]`: Массив продуктов в заказе.

### 2. Модели (models)

В папке `src/components/models` находятся модели, которые управляют данными приложения:

- **CartModel**: Управляет состоянием корзины.
  - **Свойства**:
    - `products: IProductModel[]`: Массив продуктов в корзине.
  - **Методы**:
    - `add(product: IProductModel): void`: Добавляет продукт в корзину, если он еще не существует.
    - `exist(product: IProductModel): boolean`: Проверяет, существует ли продукт в корзине.
    - `remove(id: string): void`: Удаляет продукт из корзины по его идентификатору.
    - `clearCart(): void`: Очищает корзину и эмитирует событие изменения.

- **CatalogModel**: Управляет состоянием каталога продуктов.
  - **Свойства**:
    - `products: IProductModel[]`: Массив продуктов в каталоге.
  - **Методы**:
    - `setItems(products: IProductModel[]): void`: Устанавливает продукты в каталоге и эмитирует событие обновления.
    - `getProduct(id: string): IProductModel`: Возвращает продукт по его идентификатору.

### 3. Представления (views)

В папке `src/components/view` находятся представления, которые отвечают за отображение данных и взаимодействие с пользователем:

- **CatalogView**: Отображает список продуктов в каталоге.
  - **Свойства**:
    - `container: HTMLElement`: Контейнер для отображения продуктов.
  - **Методы**:
    - `render({ products }: { products: HTMLElement[] }): HTMLElement`: Отображает переданные продукты в контейнере.

- **CartView**: Отображает содержимое корзины и общую стоимость.
  - **Свойства**:
    - `totalPrice: number`: Общая стоимость продуктов в корзине.
  - **Методы**:
    - `render(products: IProductModel[]): HTMLElement`: Отображает продукты в корзине и рассчитывает общую стоимость.

- **ProductView**: Отображает информацию о конкретном продукте.
  - **Свойства**:
    - `_events: IEventEmitter`: Эмиттер событий для обработки взаимодействий.
  - **Методы**:
    - `render(product: IProductModel): HTMLElement`: Отображает информацию о продукте и добавляет обработчик клика для эмита события просмотра продукта.

- **ProductPreviewView**: Отображает предварительный просмотр продукта в модальном окне.
  - **Свойства**:
    - `isInCart: boolean`: Указывает, находится ли продукт в корзине.
    - `_events: IEventEmitter`: Эмиттер событий для обработки взаимодействий.
  - **Методы**:
    - `render(product: IProductModel): HTMLElement`: Создает и отображает предварительный просмотр продукта, включая кнопку добавления в корзину, которая отключается, если продукт уже в корзине.

- **OrderView**: Отображает форму для оформления заказа.
  - **Свойства**:
    - `_events: IEventEmitter`: Эмиттер событий для обработки взаимодействий.
    - `products: IProductModel[]`: Массив продуктов, включенных в заказ.
    - `total: number`: Общая сумма заказа.
  - **Методы**:
    - `render(): HTMLElement`: Отображает форму для ввода информации о заказе и добавляет обработчики событий для кнопок и полей ввода.

- **ContactsView**: Отображает форму для ввода контактной информации.
  - **Свойства**:
    - `_events: IEventEmitter`: Эмиттер событий для обработки взаимодействий.
    - `orderInfo: IOrder`: Информация о заказе, включая контактные данные.
  - **Методы**:
    - `render(): HTMLElement`: Отображает форму для ввода электронной почты, телефона и адреса, а также добавляет обработчики событий для проверки заполненности полей.

- **OrderCompleteView**: Отображает сообщение об успешном завершении заказа.
  - **Свойства**:
    - `_events: IEventEmitter`: Эмиттер событий для обработки взаимодействий.
  - **Методы**:
    - `render(data: { totalPrice: number }): HTMLElement`: Отображает сообщение о том, что заказ успешно оформлен, и выводит общую сумму заказа.

- **ModalView**: Управляет отображением модальных окон.
  - **Свойства**:
    - `modalCloseButton: HTMLButtonElement`: Кнопка для закрытия модального окна.
    - `modalContainer: HTMLElement`: Контейнер для модального окна.
    - `modalContent: HTMLElement`: Контент модального окна.
    - `pageWrapper: HTMLElement`: Обертка страницы для управления состоянием модального окна.
  - **Методы**:
    - `toggle(): void`: Переключает состояние видимости модального окна.
    - `render(data: HTMLElement): HTMLElement`: Заменяет содержимое модального окна на переданные данные.

### 4. Работа модальных окон

Модальные окна используются для отображения дополнительной информации и взаимодействия с пользователем. Они создаются с помощью класса `ModalView`, который управляет их состоянием (открыто/закрыто) и содержимым. Модальные окна могут отображать различные представления, такие как `CartView`, `ProductPreviewView`, `OrderView` и `ContactsView`.

### 5. index.ts

Файл `src/index.ts` является точкой входа в приложение. В нем происходит инициализация основных компонентов приложения, таких как:

- Создание экземпляров моделей (`CartModel`, `CatalogModel`).
- Создание экземпляров представлений (`CatalogView`, `CartView`, `ModalView` и т.д.).
- Настройка обработчиков событий для взаимодействия между компонентами.
- Загрузка данных о продуктах с сервера и их отображение в каталоге.

Файл также управляет логикой, связанной с добавлением и удалением продуктов из корзины, а также оформлением заказов.

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