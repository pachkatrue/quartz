## 📋 Основная информация

- **Компонент**: Промежуточная страница поиска
- **Родительская задача**: [[search-system|Поисковая выдача]]
- **Статус**: ToDo
- **Приоритет**: High
- **ID**: SEARCH-INTERMEDIATE-001

## 🎯 Назначение

Страница уточняющего поиска для случаев, когда найдено несколько результатов из разных источников.

## 📝 Логика работы страницы

### Условия показа промежуточной страницы:

#### ✅ Показать промежуточную страницу:

1. **Множественные результаты 1С + есть Laximo**
    - В базе 1С найдено более одной запчасти
    - В Laximo найдены результаты
2. **Одна запчасть 1С + множественные Laximo**
    - В базе 1С найдена только одна запчасть
    - В Laximo найдено более одной запчасти
3. **Только множественные результаты Laximo**
    - В базе 1С ничего не найдено
    - В Laximo найдено более одной запчасти

#### ❌ НЕ показывать промежуточную страницу:

1. **Прямой переход на результаты:**
    - В базе 1С найдена только одна запчасть
    - В Laximo результатов нет
2. **Единственный результат:**
    - В Laximo найден только один результат
    - В 1С результатов нет
3. **Страница отсутствия результатов:**
    - Ни в 1С, ни в Laximo ничего не найдено

## 🔧 Функциональные требования

### Назначение страницы:

Страница должна открываться в том случае, если по запросу пользователя найдено несколько вариантов подходящих результатов. То есть не получилось однозначно определить запрос пользователя.

Страница нужна для того, чтобы однозначно определить запрос пользователя и подобрать подходящие под его запрос товары.

### Условия для семантического поиска:

#### 1️⃣ Первая очередь - поиск по номеру (полное совпадение):

- В первую очередь должен производиться поиск по номеру товара в 1С с полным совпадением
- Найденные детали выводятся первым приоритетом
- В поиске не должны учитываться: пробел, слэш, тире, точка в номере детали
- Номера "1234-4567" и "12344567" должны быть системой распознаны как идентичные

#### 2️⃣ Вторая очередь - поиск по номеру (частичное совпадение):

- Во вторую очередь должны выдаваться найденные детали по номеру с частичным совпадением
- При вводе 1234, должны выдаваться детали с номерами "1234"-"4567", "456-1234"
- Должны быть учтены очередность введенных символов и поисковой выдачи

#### 3️⃣ Третья очередь - поиск по наименованию товара:

- В третью очередь должны выдаваться найденные товары с совпадением введенного запроса в наименовании товара

#### 4️⃣ Четвертая очередь - поиск по наименованию бренда:

- В четвертую очередь должны выдаваться найденные товары с совпадением введенного запроса в наименовании бренда

### Совместный поиск по нескольким товарам:

Система поиска должна поддерживать совместный поиск по нескольким товарам. Например, при поисковом запросе "1234-567 Масляный фильтр" поиск должен производиться по номеру детали и наименованию товара. Причем расположение ключевых значений не должно влиять на поисковую выдачу. Поисковые запросы "1234-567 Масляный фильтр", "Масляный фильтр 1234-567", "Фильтр масляный 1234-567" должны обладать идентичной выдачей.

### Отображение результатов:

На странице должны быть как результаты поиска по базе 1С, так и результаты поиска по базе Laximo

#### Параметры элементов поиска:

- **Наименование товара**: Должно быть получено из 1С или из Laximo (в зависимости от источника)
- **Номер товара**: Должно быть получено из 1С или из Laximo (в зависимости от источника)
- **Бренд товара**: Должно быть получено из 1С или из Laximo (в зависимости от источника)
- **Специальное примечание**: Бренд из 1С должен быть уточнен из наименования товара (отдельного параметра в базе не заведено)

❗ **Важно**: В выгрузке 1С есть параметр "Бренд", но он не заведен.

## 🎛️ Система фильтрации

### Обязательные фильтры:

Страница должна обладать двумя фильтрами:

- Фильтры должны сохранять свое состояние при переходе между страницами
- Должна поддерживаться логика AND/OR для выбора нескольких параметров фильтров

#### 1. Фильтр "Производитель":

- **Логика**: Должен обладать списком чекбоксов производителей товаров полученных по выбранной подкатегории из 1С
- **Сортировка**: По умолчанию список производителей должен сортироваться в прямом алфавитном порядке
- **Ограничение**: Должно выводиться до 10 доступных производителей
- **Расширение**: Если брендов оказалось больше, то в блоке фильтра должна отображаться кнопка пагинации "Показать ещё"

#### 2. Фильтр "Категория":

- **Описание**: Категория - это список категорий последнего уровня, к которым относятся найденный товары
- **Логика**: Должен обладать списком чекбоксов категорий последнего уровня, которые принадлежат к товарам поисковой выдачи
- **Сортировка**: По умолчанию список категорий должен сортироваться в прямом алфавитном порядке
- **Ограничение**: Должно выводиться до 10 доступных категорий
- **Расширение**: Если категорий оказалось больше, то в блоке фильтра должна отображаться кнопка пагинации "Показать ещё"

### Поисковая строка в фильтрах:

- **Функционал**: Фильтр должен обладать поисковой строкой
- **Логика работы**: Строка поиска должна работать только по этому блоку
- **Условие поиска**: Поиск по введенному запросу должен производиться после ввода первого символа
- **Пустые результаты**: Если подходящих результатов нет, то блок с брендами должен стать пустым
- **Сброс**: Если инпут пустой, то запрос на поиск не производится, должны выводиться значения по-умолчанию

### Управление фильтрами:

- **Иконка "Удалить"**: Должна удалять введенные символы в инпут. Список должен возвращаться в исходное состояние
- **Появление иконки**: Иконка должна появляться при вводе 1-го символа

#### Скрытие фильтров:

- **Условие**: Фильтр не должен выводиться, если в результате поиска был найден только один производитель
- **Кнопка "Применить фильтр"**: По клику на кнопку список товаров должен отфильтроваться по заданным в фильтры параметрам
- **Неактивное состояние**: Если значения в фильтре не выбраны, то кнопка должна быть задизейблена

### Кнопка "Сбросить фильтры":

- **Функционал**: По клику на кнопку примененные фильтры должны очиститься, список товаров должен принять изначальное состояние
- **Неактивное состояние**: Если значения в фильтре не выбраны, то кнопка должна быть задизейблена

### Производительность:

- **Время ответа**: Результаты поиска должны выдаваться с минимальной задержкой, не более 3 секунд

### Структура страницы:

```
┌─────────────────────────────────────────────────────────┐
│ 🔍 Результаты поиска по: "запрос"                       │
├─────────────────────────────────────────────────────────┤
│ 📊 Найдено результатов: X                               │
├─────────────────────────┬───────────────────────────────┤
│ 🎛️ ФИЛЬТРЫ              │ 📋 РЕЗУЛЬТАТЫ ПОИСКА          │
│                         │                               │
│ 🏭 Производитель        │ 🥇 По номеру (полное совпад.) │
│ ┌─────────────────────┐ │ ┌───────────────────────────┐ │
│ │ 🔍 [поиск]          │ │ │ [Список результатов 1С]   │ │
│ │ ☐ Bosch (15)        │ │ └───────────────────────────┘ │
│ │ ☐ Toyota (8)        │ │                               │
│ │ ☐ Mann (5)          │ │ 🥈 По номеру (частичное)      │
│ │ [Показать ещё]      │ │ ┌───────────────────────────┐ │
│ └─────────────────────┘ │ │ [Список результатов 1С]   │ │
│                         │ └───────────────────────────┘ │
│ 🏷️ Категория            │                               │
│ ┌─────────────────────┐ │ 🥉 По наименованию товара     │
│ │ 🔍 [поиск]          │ │ ┌───────────────────────────┐ │
│ │ ☐ Фильтры (12)      │ │ │ [Список результатов 1С]   │ │
│ │ ☐ Тормоза (9)       │ │ └───────────────────────────┘ │
│ │ ☐ Масла (7)         │ │                               │
│ │ [Показать ещё]      │ │ 🏅 По наименованию бренда     │
│ └─────────────────────┘ │ ┌───────────────────────────┐ │
│                         │ │ [Список результатов 1С]   │ │
│ [Применить фильтр]      │ └───────────────────────────┘ │
│ [Сбросить фильтры]      │                               │
│                         │ 🌐 Результаты Laximo          │
│                         │ ┌───────────────────────────┐ │
│                         │ │ [Внешние результаты]      │ │
│                         │ └───────────────────────────┘ │
└─────────────────────────┴───────────────────────────────┘
```

## 🎨 UX/UI компоненты

### Карточка результата из 1С:

```
┌─────────────────────────────────────┐
│ 🔧 Название запчасти                │
│ 🏷️ Бренд: Toyota                   │
│ 📦 Номер: 12345-ABC                │
│ 💰 Цена: 1 500 ₽                   │
│ 📊 В наличии: 5 шт.                │
│ [Подробнее] [В корзину]             │
└─────────────────────────────────────┘
```

### Карточка результата из Laximo:

```
┌─────────────────────────────────────┐
│ 🔧 Название запчасти                │
│ 🏷️ Бренд: Bosch                    │
│ 📦 Номер: 67890-XYZ                │
│ 🌐 Внешний каталог                  │
│ [Найти аналоги] [Заказать]          │
└─────────────────────────────────────┘
```

## 📊 Алгоритм принятия решений

### Блок-схема логики:

```
Получен поисковый запрос
        ↓
Поиск в базе 1С
        ↓
┌─────────────────────┐
│ Найдено в 1С?       │
└─────────────────────┘
    ↓ Да        ↓ Нет
┌─────────┐  ┌─────────┐
│ 1 шт.?  │  │ Поиск в │
└─────────┘  │ Laximo  │
    ↓        └─────────┘
┌─────────┐      ↓
│ Поиск в │  ┌─────────┐
│ Laximo  │  │ Найдено?│
└─────────┘  └─────────┘
    ↓            ↓
┌─────────┐  ┌─────────┐
│ Есть    │  │ Показать│
│ результ?│  │ "Ничего"│
└─────────┘  └─────────┘
    ↓
┌─────────────────────┐
│ Показать            │
│ промежуточную       │
│ страницу?           │
└─────────────────────┘
```

## 🔧 Технические требования

### Frontend компоненты:

- [ ] **SearchResultsPage** - основной компонент страницы
- [ ] **SearchFilters** - блок фильтрации
- [ ] **ProducerFilter** - фильтр производителей
- [ ] **CategoryFilter** - фильтр категорий
- [ ] **SearchableFilter** - поисковая строка в фильтрах
- [ ] **ResultsGrouped** - группированные результаты
- [ ] **ResultCard1C** - карточка результата из 1С
- [ ] **ResultCardLaximo** - карточка результата из Laximo

### Алгоритм поиска:

```javascript
searchAlgorithm = {
  priority1: "exact_number_match",    // Точное совпадение номера
  priority2: "partial_number_match",  // Частичное совпадение номера  
  priority3: "product_name_match",    // Совпадение в названии
  priority4: "brand_name_match",      // Совпадение в бренде
  
  // Нормализация номера (убираем спецсимволы)
  normalizeNumber: (number) => {
    return number.replace(/[-.\s/]/g, '');
  },
  
  // Совместный поиск по нескольким параметрам
  combinedSearch: true,
  positionIndependent: true  // Порядок слов не важен
}
```

### API endpoints:

```javascript
// Поиск с приоритетами
GET /api/search/intermediate?q={query}&priorities=true

// Фильтрация производителей
GET /api/filters/producers?category={cat}&limit=10&offset=0

// Фильтрация категорий  
GET /api/filters/categories?query={q}&limit=10&offset=0

// Поиск в фильтрах
GET /api/filters/search?type={producer|category}&q={query}
```

### Состояние фильтров:

```javascript
filterState = {
  producers: {
    selected: [],
    visible: 10,
    searchQuery: "",
    total: 0
  },
  categories: {
    selected: [],
    visible: 10, 
    searchQuery: "",
    total: 0
  },
  applied: false,
  // Сохранение состояния между страницами
  persistent: true
}
```

### Логика AND/OR:

- **AND**: Товары должны соответствовать всем выбранным фильтрам
- **OR**: Внутри одного фильтра - любое из выбранных значений
- Пример: (Producer1 OR Producer2) AND (Category1 OR Category2)

## 🔗 Связанные компоненты

- [[search-system|Поисковая выдача]] - основная логика
- [[Страница результатов поиска]] - финальная страница
- [[Страница отсутствия результатов]] - альтернативный путь
- [[API каталога 1С]] - источник данных
- [[API Laximo]] - внешний источник

## 🧪 Сценарии тестирования

### Тест-кейсы:

1. **TC-001**: Поиск находит 1 результат в 1С + 0 в Laximo
    - Ожидаемый результат: Прямой переход на страницу результатов
2. **TC-002**: Поиск находит 1 результат в 1С + 3 в Laximo
    - Ожидаемый результат: Показать промежуточную страницу
3. **TC-003**: Поиск находит 0 результатов в 1С + 1 в Laximo
    - Ожидаемый результат: Прямой переход на страницу результатов
4. **TC-004**: Поиск находит 0 результатов в 1С + 0 в Laximo
    - Ожидаемый результат: Страница "Ничего не найдено"

### Граничные случаи:

- [ ] Тайм-аут запроса к внешнему API
- [ ] Ошибка соединения с базой 1С
- [ ] Некорректный формат ответа от Laximo
- [ ] Слишком большое количество результатов (>100)

---

#промежуточная-страница #логика-поиска #результаты #1с #laximo