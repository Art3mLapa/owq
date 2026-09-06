# WindUI API Documentation

Комплексная документация по всем методам, конфигурациям и компонентам библиотеки **WindUI** (включая все новые доработки: заставку OWQ, браузерное позиционирование вкладок `Top`/`Bottom`, обновленный `OpenButton` и центрированный заголовок).

---

## Оглавление
1. [Подключение библиотеки](#1-подключение-библиотеки)
2. [Главный объект `WindUI`](#2-главный-объект-windui)
3. [Создание окна `WindUI:CreateWindow`](#3-создание-окна-winduicreatewindow)
4. [Методы Window](#4-методы-window)
5. [Заставка OWQ (Splash Animation)](#5-заставка-owq-splash-animation)
6. [Кнопка свернутого меню (OpenButton)](#6-кнопка-свернутого-меню-openbutton)
7. [Вкладки (Tabs) и Секции Вкладок](#7-вкладки-tabs-и-секции-вкладок)
8. [Элементы интерфейса (Elements)](#8-элементы-интерфейса-elements)
   * [Button (Кнопка)](#button-кнопка)
   * [Toggle (Тумблер / Чекбокс)](#toggle-тумблер--чекбокс)
   * [Slider (Слайдер / Ползунок)](#slider-слайдер--ползунок)
   * [Input (Поле ввода)](#input-поле-ввода)
   * [Dropdown (Выпадающий список)](#dropdown-выпадающий-список)
   * [Colorpicker (Выбор цвета)](#colorpicker-выбор-цвета)
   * [Keybind (Назначение клавиши)](#keybind-назначение-клавиши)
   * [Paragraph (Информационный блок)](#paragraph-информационный-блок)
   * [ProgressBar (Индикатор прогресса)](#progressbar-индикатор-прогресса)
   * [Code (Блок кода)](#code-блок-кода)
   * [Image & Video & Viewport](#image--video--viewport)
   * [Layout элементы: Section, Group, HStack, VStack, Space, Divider](#структурные-элементы)
9. [Диалоговые окна и всплывающие уведомления](#9-диалоговые-окна-и-всплывающие-уведомления)
   * [Dialog](#dialog-модальное-окно)
   * [Popup](#popup-всплывающее-окно)
   * [Notify (Уведомления)](#notify-уведомления)
10. [Система конфигураций (ConfigManager)](#10-система-конфигураций-configmanager)
11. [Система тем (Themes)](#11-система-тем-themes)

---

## 1. Подключение библиотеки

### Загрузка из Potassium Workspace (локально):
```lua
local WindUI

if isfile and isfile("WindUI.lua") then
    local chunk, err = loadfile("WindUI.lua")
    if chunk then
        WindUI = chunk()
    else
        warn("[WindUI Load Error]: " .. tostring(err))
    end
elseif readfile and isfile and isfile("WindUI.lua") then
    WindUI = loadstring(readfile("WindUI.lua"))()
else
    WindUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/Footagesus/WindUI/main/dist/main.lua"))()
end
```

---

## 2. Главный объект `WindUI`

Глобальные методы библиотеки:

| Мтод | Параметры | Описание |
| :--- | :--- | :--- |
| `WindUI:CreateWindow(Config)` | `Config: table` | Создает и возвращает главное окно GUI. |
| `WindUI:Notify(Config)` | `Config: table` | Отправляет всплывающее уведомление на экран. |
| `WindUI:Popup(Config)` | `Config: table` | Открывает модальное всплывающее окно `Popup`. |
| `WindUI:Splash(onComplete)` | `onComplete: function?` | Воспроизводит кинематографичную анимацию заставки OWQ. |
| `WindUI:SetTheme(ThemeName)` | `ThemeName: string` | Меняет текущую активную тему (`"Dark"`, `"Light"`, `"Mellowsi"` и др.). |
| `WindUI:AddTheme(ThemeTable)` | `ThemeTable: table` | Регистрирует новую пользовательскую тему. |
| `WindUI:GetCurrentTheme()` | — | Возвращает имя текущей темы. |
| `WindUI:GetThemes()` | — | Возвращает таблицу всех доступных тем. |
| `WindUI:SetFont(FontId)` | `FontId: string` | Меняет шрифт всей библиотеки на кастомный. |
| `WindUI:SetNotificationLower(val)` | `val: boolean` | Смещает контейнер уведомлений ниже. |

---

## 3. Создание окна `WindUI:CreateWindow`

```lua
local Window = WindUI:CreateWindow({
    Title = ".ftgs hub | WindUI",  -- Заголовок (всегда строго по центру)
    Author = "by .ftgs • Footagesus", -- Автор/Подзаголовок (строго под заголовком)
    Folder = "MyScriptHub",        -- Папка для сохранения конфигов и ассетов
    Icon = "solar:folder-2-bold-duotone", -- Иконка окна (левый угол)
    IconSize = 22,                 -- Размер иконки
    Theme = "Dark",                -- Тема по умолчанию
    NewElements = true,            -- Использовать обновленный стиль скруглений
    Size = UDim2.fromOffset(600, 460), -- Начальный размер
    MinSize = Vector2.new(560, 350),   -- Минимальный размер при ресайзе
    MaxSize = Vector2.new(850, 600),   -- Максимальный размер при ресайзе
    Resizable = true,              -- Разрешить менять размер перетаскиванием за угол
    
    -- [НОВОЕ] Настройки вкладок:
    TabPosition = "Top",           -- Положение вкладок: "Top" (сверху) или "Bottom" (снизу)
    TabBarHeight = 38,             -- Высота панели вкладок (по умолчанию 38px)
    
    -- [НОВОЕ] Заставка OWQ:
    Splash = true,                 -- Включить/выключить заставку при запуске окна (по умолчанию true)

    -- Поиск:
    HideSearchBar = false,         -- Показывать/прятать кнопку поиска

    -- [НОВОЕ] Кнопка свернутого меню:
    OpenButton = {
        Title = "OWQ",             -- Текст кнопки
        Enabled = true,            -- Включена ли кнопка
        Draggable = true,          -- Разрешить перемещать за нижнюю подложку
        OnlyMobile = false,        -- Показывать только на мобильных (false = везде)
        Scale = 1,                 -- Масштаб кнопки
        Width = 72,                -- Ширина кнопки
        Height = 32,               -- Высота кнопки
    },

    Topbar = {
        Height = 44,
        ButtonsType = "Mac",       -- Стиль кнопок: "Mac" (цветные кружки) или "Default"
    },
})
```

---

## 4. Методы Window

Объект `Window`, возвращаемый функцией `CreateWindow`:

```lua
Window:Open()                     -- Разворачивает окно
Window:Close()                    -- Сворачивает окно (показывает OpenButton)
Window:Toggle()                   -- Переключает состояние открыто/закрыто
Window:Destroy()                  -- Полностью уничтожает GUI
Window:SetToTheCenter()           -- Центрирует окно по экрану
Window:SetSize(UDim2)             -- Устанавливает новый размер окна
Window:SetTitle("New Title")      -- Меняет заголовок
Window:SetAuthor("New Author")    -- Меняет автора
Window:SetTabPosition("Top" | "Bottom") -- [НОВОЕ] Плавно переносит вкладки наверх или вниз
Window:Tag({ Title = "v1.0" })    -- Добавляет информационный тег в Topbar
Window:SelectTab(1)               -- Программно выбирает вкладку по индексу
Window:LockAll()                  -- Блокирует все элементы в окне
Window:UnlockAll()                -- Разблокирует все элементы
Window:SetUIScale(0.9)            -- Задает общий масштаб интерфейса
Window:OnOpen(function() end)     -- Соытие при открытии
Window:OnClose(function() end)    -- Событие при закрытии
Window:OnDestroy(function() end)  -- Событие при удалении
```

---

## 5. Заставка OWQ (Splash Animation)

При вызове `WindUI:CreateWindow` (если не задан `Splash = false`) или ручном вызове `WindUI:Splash(callback)` проигрывается фирменная анимация:
1. **Камера и затемнение:** Плавный зум камеры (уменьшение FOV на 14 градусов), сильное размытие мира (`BlurEffect`) и мягкое затемнение экрана через `EasingStyle.Exponential`.
2. **Логотип OWQ:** Текст `"owq"` со стилем **Segoe UI Light** плавно выезжает сверху в центр экрана.
3. **Хвостик буквы Q:** Хвостик буквы `q` через `EasingStyle.Cubic` вытягивается до самого нижнего края экрана.
4. **Сворачивание:** Хвостик втягивается обратно, логотип уменьшается и растворяется первым.
5. **Бесшовный переход:** Сразу в момент исчезновения текста окно начинает раскрыаться, пока блюр и камера плавно возвращаются в исходное состояние.

```lua
-- Пример ручного вызова:
WindUI:Splash(function()
    print("Заставка завершилась!")
end)
```

---

## 6. Кнопка свернутого меню (OpenButton)

Когда окно сворачивается, на экране появляется кнопка `"OWQ"`:
* **Дизайн:** Оформлена в стиле самого окна (`Squircle` + рамка `Glass-1.4`).
* **Подложка перетаскивания:** Снизу кнопки расположена подложка-хэндл. Взявшись за нее, кнопку можно переместить в любое удобное место экрана.
* **Клик по кнопке:** Моментально разворачивает окно интерфейса.

---

## 7. Вкладки (Tabs) и Секции Вкладок

### Создание вкладки:
```lua
local Tab = Window:Tab({
    Title = "Main Tab",
    Icon = "solar:home-2-bold",   -- Иконка вкладки
    IconColor = Color3.fromHex("#30ff6a"), -- Цвет иконки (опционально)
    IconShape = "Square",         -- "Square", "Circle" или nil
    Border = true,                -- Стеклянная рамка
    Locked = false,               -- Заблокирована ли вкладка
    Desc = "Описание вкладки при наведении (Tooltip)",
})
```

**Методы вкладки:**
* `Tab:Select()` — сделать вкладку активной.
* `Tab:LockAll()` / `Tab:UnlockAll()` — заблокировать/разблокировать элементы во вкладке.

### Секция вкладок (Выпадающая группа во вкладках):
```lua
local Section = Window:Section({
    Title = "Category Name",
    Opened = true,                -- Раскрыта ли секция сразу
})

-- Добавление вкладок внутрь секции:
local SubTab = Section:Tab({
    Title = "Sub-Feature",
    Icon = "box",
})
```

---

## 8. Элементы интерфейса (Elements)

Все элементы добавляются вызовом метода у объекта вкладки `Tab:ElementName({ ... })`, группы `Group:ElementName({ ... })` или секции.

---

### Button (Кнопка)
```lua
local MyButton = Tab:Button({
    Title = "Execute Script",
    Desc = "Нажмите для запуска функции",
    Icon = "play",
    Color = Color3.fromHex("#305dff"), -- ользовательский цвет
    Justify = "Between",               -- "Between" или "Center"
    Callback = function()
        print("Кнопка нажата!")
    end,
})

MyButton:SetTitle("New Button Title")
MyButton:SetDesc("New Description")
MyButton:Lock()   -- Заблокировать кнопку
MyButton:Unlock() -- Разблокировать кнопку
```

---

### Toggle (Тумблер / Чекбокс)
```lua
local MyToggle = Tab:Toggle({
    Title = "Speedhack",
    Desc = "Увеличивает скорость перемещения",
    Type = "Toggle",                   -- "Toggle" (тумблер) или "Checkbox" (галочка)
    Value = false,                     -- Начальное состояние
    Flag = "Toggle_Speed",             -- Ключ для ConfigManager
    Callback = function(state)
        print("Состояние:", state)
    end,
})

MyToggle:Set(true) -- Программно изменить значение
```

---

### Slider (Слайдер / Ползунок)
```lua
local MySlider = Tab:Slider({
    Title = "WalkSpeed",
    Desc = "Настройка скорости персонажа",
    Step = 1,                          -- Шаг изменения
    Value = {
        Min = 16,
        Max = 250,
        Default = 16,
    },
    IsTooltip = true,                  -- Показывать текущее значение в тултипе
    IsTextbox = false,                 -- Поле ввода числа вручную
    Icons = {
        From = "sfsymbols:figure.walk",
        To = "sfsymbols:figure.run",
    },
    Flag = "Slider_WalkSpeed",
    Callback = function(val)
        print("Значение слайдера:", val)
    end,
})

MySlider:Set(100) -- Программно изменить значение
```

---

### Input (Поле ввода)
```lua
local MyInput = Tab:Input({
    Title = "Player Target",
    Desc = "Введите ник игрока",
    Type = "Input",                    -- "Input" (одна строка) или "Textarea" (многострочный)
    Value = "DefaultText",
    Placeholder = "Никнейм...",
    Flag = "Input_Target",
    Callback = function(text)
        print("Введен текст:", text)
    end,
})

MyInput:Set("NewText")
```

---

### Dropdown (Выпадающий список)
```lua
local MyDropdown = Tab:Dropdown({
    Title = "Select Weapon",
    Values = { "Sword", "Bow", "Magic Staff" },
    Value = "Sword",                   -- Значение по умолчанию
    Multi = false,                     -- true для мульти-выбора нескольких значений
    AllowNone = false,                 -- Разрешить снять все галочки
    Flag = "Dropdown_Weapon",
    Callback = function(selected)
        print("Выбрано:", selected)
    end,
})

MyDropdown:Refresh({ "Item 1", "Item 2" }) -- Обновить список вариантов
MyDropdown:Select("Item 2")                -- Выбрать программно
```

---

### Colorpicker (Выбор цвета)
```lua
local MyColor = Tab:Colorpicker({
    Title = "ESP Color",
    Desc = "Цвет подсветки",
    Default = Color3.fromRGB(255, 0, 0),
    Transparency = 0,                  -- Поддержка прозрачности (0 - 1)
    Flag = "Color_ESP",
    Callback = function(color, transparency)
        print("Выбран цвет:", color, "Прозрачность:", transparency)
    end,
})

MyColor:Set(Color3.fromRGB(0, 255, 0), 0.2)
```

---

### Keybind (Назначение клавиши)
```lua
local MyBind = Tab:Keybind({
    Title = "Teleport Key",
    Desc = "Нажмите для бинда",
    Value = "E",                       -- Клавиша по умолчанию
    Flag = "Keybind_TP",
    Callback = function(keyCode)
        print("Сработал бинд:", keyCode)
    end,
})
```

---

### Paragraph (Информационный блок)
```lua
Tab:Paragraph({
    Title = "Информация",
    Desc = "Подробное описание модуля или правила скрипта.",
    Image = "solar:info-circle-bold",
    ImageSize = 32,
    Buttons = {
        {
            Title = "Скопировать",
            Icon = "copy",
            Callback = function()
                setclipboard("text")
            end,
        }
    }
})
```

---

### ProgressBar (Индикатор прогресса)
```lua
local Bar = Tab:ProgressBar({
    Title = "Загрузка данных",
    Progress = 50, -- 0..100
})

Bar:Set(85) -- Обновить прогресс
```

---

### Code (Блок кода)
```lua
Tab:Code({
    Title = "Lua Script",
    Code = "print('Hello from WindUI')",
})
```

---

### Image & Video & Viewport
```lua
-- Картинка:
Tab:Image({
    Image = "rbxassetid://12345678", -- или http ссылка
    AspectRatio = "16:9",
    Radius = 9,
})

-- 3D Viewport:
Tab:Viewport({
    Title = "3D Preview",
    Model = game.Workspace.Part,
})
```

---

### Структурные элементы

| Элемент | Описание | Пример вызова |
| :--- | :--- | :--- |
| `Tab:Space({ Columns = 2 })` | Пустой вертикальный отступ | `Tab:Space()` |
| `Tab:Divider()` | Тонкая горизонтальная линия-разделитель | `Tab:Divider()` |
| `Tab:Group()` | Объединяет несколько элементов в цельную плашку | `local Group = Tab:Group({})` |
| `Tab:HStack()` | Горизонтальный ряд элементов | `local Row = Tab:HStack({})` |
| `Tab:VStack()` | Вертикальный стек элементов | `local Col = Tab:VStack({})` |

---

## 9. Диалоговые окна и всплывающие уведомления

### Dialog (Модальное окно)
```lua
Window:Dialog({
    Title = "Подтверждение",
    Content = "Вы уверены, что хотите выполнить это действие?",
    Width = 340,
    Buttons = {
        {
            Title = "Отмена",
            Variant = "Secondary",
            Callback = function() end,
        },
        {
            Title = "Да",
            Variant = "Primary",
            Callback = function()
                print("Подтверждено!")
            end,
        },
    }
})
```

### Popup (Всплывающее окно)
```lua
WindUI:Popup({
    Title = "Внимание",
    Icon = "alert-triangle",
    Content = "Скрипт успешно запущен и готов к работе!",
    Buttons = {
        {
            Title = "Понятно",
            Variant = "Primary",
        }
    }
})
```

### Notify (Уведомления)
```lua
WindUI:Notify({
    Title = "Успешно!",
    Content = "Настройки сохранены в конфиг.",
    Icon = "check",
    Duration = 5,                      -- Время показа в секундах
    CanClose = true,                   -- Кнопка закрытия
})
```

---

## 10. Система конфигураций (ConfigManager)

Если в `CreateWindow` указано поле `Folder = "ИмяПапки"`, библиотека автоматически инициализирует менеджер конфигов:

```lua
local ConfigManager = Window.ConfigManager

-- Создать и сохранить конфиг:
local myConfig = ConfigManager:CreateConfig("default")
myConfig:Save()

-- Загрузить конфиг:
myConfig:Load()

-- Получить список всех сохраненных конфигов:
local all = ConfigManager:AllConfigs()
```
Все элементы с заданным свойством `Flag = "УникальныйКлюч"` автоматически попадают в сохранение и загрузку!

---

## 11. Система тем (Themes)

В библиотеку встроены светлые и тёмные темы: `"Dark"`, `"Light"`, `"Mellowsi"` и др.

### Создание своей темы:
```lua
WindUI:AddTheme({
    Name = "MyTheme",
    WindowBackground = Color3.fromHex("#121214"),
    PanelBackground = Color3.fromHex("#18181b"),
    Text = Color3.fromHex("#ffffff"),
    TabBackground = Color3.fromHex("#27272a"),
    TabBackgroundActive = Color3.fromHex("#3f3f46"),
    WindowTopbarTitle = Color3.fromHex("#fafafa"),
    WindowTopbarAuthor = Color3.fromHex("#a1a1aa"),
})

WindUI:SetTheme("MyTheme")
```
