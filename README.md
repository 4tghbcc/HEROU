# HEROU
C
Проект: Практическая реализация и оценка пользовательского интерфейса (Unity, стратегия в духе «Герои»)

Назначение
- Основная цель — создать модульный, адаптивный и расширяемый UI для стратегии с изометрической графикой на Unity, который:
  * обеспечивает целостный пользовательский опыт на ПК и планшетах (hover + touch), поддерживает DPI/Retina, breakpoints и 9‑slice масштабирование;
  * реализует единый визуальный стиль с темами (дневная/ночная), кастомизируемыми панелями и унифицированной сеткой отступов;
  * использует событийную архитектуру (делегаты/шина событий), паттерны MVC/MVP, командный подход для отката действий;
  * включает мультиязычность (ключи-идентификаторы + внешние файлы локализации), доступность (масштаб шрифтов, высокий контраст, озвучивание ключевых действий);
  * интегрирован с аналитикой (Unity Analytics), телеметрией, автотестами и A/B‑экспериментами для оценки эффективности;
  * поддерживает быстрые действия и горячие клавиши с пользовательской перенастройкой.

- Ключевые зоны UI: верхняя панель ресурсов, миникарта, боковые панели, списки заданий, нижняя панель героев/армии, всплывающие окна и универсальный Tooltip.

- Результаты UX‑оценки: повышение скорости освоения, снижение числа ошибок, подтвержденная удобочитаемость и последовательность навигации после итераций тестирования.

Структура проекта
- Директории верхнего уровня:
  * `Assets/` — контент и код проекта.
    + `Scenes/`
      - `Bootstrap.unity` — точка входа (инициализация сервисов, тем, локализации).
      - `WorldMap.unity`, `UI_Playground.unity` — основная и демо-сцены UI.
    + `Scripts/`
      - `UI/`
        • `Core/` — `UIManager`, `WindowController`, `NavigationService`, `TooltipService`.
        • `Widgets/` — `ResourceBar`, `MiniMap`, `SidePanel`, `QuestList`, `HeroBar`, `ModalWindow`.
        • `Themes/` — `ThemeManager`, `ThemeData` (ScriptableObject), переключатели дневной/ночной темы.
        • `Localization/` — `LocalizationProvider`, загрузка строк по ключам.
        • `Accessibility/` — `AccessibilityManager` (контраст, шрифты, анимации).
        • `Commands/` — `CommandBus` и команды UI (свернуть панели, сменить стиль миникарты и т. п.).
        • `Analytics/` — интерфейсы и адаптеры: `UnityAnalyticsProvider`, `DummyAnalyticsProvider`.
      - `Game/`
        • `Models/` — `Hero`, `Army`, `Resource`, `Quest`, состояния игры.
        • `Systems/` — `GameState`, `TimeOfDay`, `Input` (PC/Touch), контроллеры (например, `CartController`).
    + `Art/UI/`
      - `Sprites/`, `Atlases/` (Sprite Atlas V2 для снижения draw calls), `9Slice/` рамки и панели.
    + `Shaders/UI/` — шейдеры для масок, градиентов, состаренных краев.
    + `Prefabs/UI/`
      - `Canvases/` — `RootCanvas` с уровнями: `HUD`, `Panels`, `Modals`, `System` (tooltips/toasts).
      - `Windows/`, `Panels/`, `Controls/` — переиспользуемые префабы (Prefab Variants).
    + `Resources/Localization/`
      - `ru/`, `en/` — файлы локализации (CSV/JSON), словарь подсказок.
      - `AppConfig.asset` — флаги и дефолтные настройки (тема, язык, провайдер аналитики).
    + `Animations/` — анимации UI (Animator Controllers, clips), `AvatarMasks/`.
    + `Tests/`
      - `EditMode/`, `PlayMode/` — автотесты (проверки верстки, событий, tooltips).
    + `Editor/Tools/` — автокликеры, профайлер UI, утилиты сборки атласов/адресуемых.
  * `ProjectSettings/`, `Packages/` — конфигурация Unity и пакетов.

- Иерархия UI (внутри `RootCanvas`):
  * `Background` → `HUD (TopBar, MiniMap)`
  * `Panels (SidePanels, QuestList, HeroBar)`
  * `Modals (Dialogs, Inventory, Settings)`
  * `System (Tooltips, Toasters, Overlays)`

- Основные зависимости:
  * Unity 2022.3 LTS (рекомендуется), `UGUI`, `TextMeshPro`, `Input System`, Sprite Atlas V2, Addressables (по желанию), Unity Analytics (опционально).
  * Git LFS для хранения графических ассетов.

Как запустить
- Системные требования:
  * Windows 10+/macOS 12+, 8 ГБ ОЗУ (рекомендовано 16+), GPU с поддержкой DX11/Metal.
  * Unity Hub + Unity 2022.3 LTS.

- Клонирование репозитория (с поддержкой LFS):
  *     git lfs install
    git clone https://github.com/<org>/<repo>.git
    
  * Откройте папку проекта через Unity Hub (версия редактора — 2022.3 LTS).

- Первая настройка в редакторе:
  * Импортируйте ресурсы TMP: Window → TextMeshPro → Import TMP Essential Resources.
  * Активируйте новый `Input System`: Edit → Project Settings → Player → Active Input Handling → Both.
  * Убедитесь, что включены Sprite Atlas V2: Edit → Project Settings → Editor → Sprite Packer → Enabled (Always).

- Сборка ассетов и сцен:
  * Атласы спрайтов: Menu → Assets → Rebuild Sprite Atlas (или авто при плее).
  * Addressables (если используются): Window → Asset Management → Addressables → Build → New Build.
  * Build Settings: добавьте сцены `Bootstrap.unity`, `WorldMap.unity`, `UI_Playground.unity` в список.

- Запуск из редактора:
  * Откройте `Assets/Scenes/Bootstrap.unity` и нажмите Play.
  * Для быстрого ознакомления откройте `UI_Playground.unity` (демо основных виджетов).

- Переключение темы, языка и доступности:
  * В рантайме — кнопка Settings в верхней панели.
  * По умолчанию — измените `Resources/AppConfig.asset` (тема, язык, провайдер аналитики, размер шрифта, скорость анимаций).

- Аналитика и телеметрия:
  * Без внешних сервисов: установите в `AppConfig` провайдер `DummyAnalyticsProvider`.
  * С Unity Analytics: Services → Analytics → Enable, свяжите проект с Unity Cloud Project ID, в `AppConfig` выберите `UnityAnalyticsProvider`.

- Запуск автотестов:
  * Через редактор: Window → Test Runner → EditMode/PlayMode → Run All.
  * Через CLI (CI/CD):
    -       "/Applications/Unity/Hub/Editor/2022.3.XXf1/Unity.app/Contents/MacOS/Unity" 
        -batchmode -quit -projectPath "$(pwd)" -runTests 
        -testPlatform playmode -logFile test.log
      

- Сборка билда:
  * File → Build Settings → выберите платформу (PC/Mac/Linux), добавьте сцены, нажмите Build.

- Частые проблемы и решения:
  * Нет шрифтов/иконки TMP — повторно импортируйте TMP Essential Resources.
  * Розовые элементы UI — проверьте шейдеры и графический пайплайн, обновите материалы в `Shaders/UI/`.
  * Конфликт ввода (мышь/тач) — установите `Active Input Handling: Both`, перезапустите редактор.
  * Предупреждения аналитики — переключите провайдера на `DummyAnalyticsProvider` для локального запуска.

Этого описания достаточно, чтобы:
- понять назначение системы и принципы архитектуры;
- быстро ориентироваться в структуре каталогов и ключевых модулей;
- развернуть проект локально, запустить демо-сцену, включить/выключить аналитику, провести тесты и собрать билд.
