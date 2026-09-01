<div align="center">

# 🔋 Flyme Battery Keeper

**Magisk module that softens background app killing on Flyme 11**

[English](#english)|[Русский](#русский)

</div>

---

<a id="english"></a>
## 🇬🇧 English

### What it does

Flyme 11 (Meizu/Flyme OS) is notoriously aggressive at killing background apps, which breaks notifications for messengers, music apps, and trackers. **Flyme Battery Keeper** relaxes those limits to a balanced middle ground — noticeably more permissive than stock Flyme, but not so loose that it drains your battery.

This is **not** an "unlimited background apps" module. Every value was chosen to trade a *small* amount of battery for a *large* improvement in app reliability, without the extremes that cause thermal/CPU issues or full battery drain.

### Tested on

- **Meizu 21**, Flyme 11
- Qualcomm Snapdragon 8 Gen 3

It may work on other Qualcomm-based Flyme devices, but tuning was done specifically for this chipset generation.

### What it changes

| Area | Change |
|---|---|
| Qualcomm background app limit | Raised from stock to **40** processes |
| Qualcomm cached/trim thresholds | Balanced (`empty_app_percent=85`, `trim_cache_percent=85`, etc.) |
| LMKD (Low Memory Killer) | Slightly relaxed pressure levels (`800/700/600`) — not disabled |
| Max cached processes | Raised to **150** (stock Flyme: 32) |
| Phantom process killer | Left **enabled** (softened via the above, not disabled outright) |
| Wi‑Fi / BLE background scanning | Disabled — these are real, hidden battery drains largely unrelated to "app killing" |
| Adaptive Battery | **Untouched** — kept on, unlike earlier versions of this module |
| Protected apps | Optional Doze whitelist + `oom_score_adj` boost for packages you list |

### Why these values

- **`bg_apps_limit=40`** — meaningfully higher than Flyme's stock limit, but not "everything runs forever," which would itself hurt battery life through constant memory pressure and background CPU work.
- **LMKD levels (`800/700/600`)** instead of the common "set everything to 1001" trick — setting everything to 1001 stops the system from ever trimming memory, which paradoxically triggers heavier full garbage-collection/swap cycles and *hurts* battery life. This module lets the system breathe.
- **`max_cached_processes=150`** instead of an extreme 256 — keeps the apps you care about resident in memory without inflating RAM/CPU usage to the point where the system starts thrashing.
- **Wi-Fi/BLE always-scan disabled** — continuous background scanning for location purposes is one of the most significant, and most overlooked, sources of battery drain — and it has nothing to do with background app limits.
- **Adaptive Battery left on** — unlike an earlier version of this module, this build doesn't disable Android's built-in charging/usage optimization.

### Requirements

- A rooted device with **Magisk** (or a compatible fork, e.g. KernelSU/APatch with Magisk module support)
- Flyme 11 on a Qualcomm Snapdragon 8 Gen 3 device (other configurations: use at your own risk)

### Installation

1. Download the module ZIP from the [Releases](../../releases) page (or clone/zip this repo).
2. Open **Magisk** → **Modules** → **Install from storage**.
3. Select the ZIP file and confirm installation.
4. **Reboot** your device.

### Configuration — protecting specific apps

By default, no app is force-protected — the module just relaxes system-wide limits. If you want to keep specific apps (e.g. a messenger) alive more aggressively, edit `service.sh` before flashing (or via a root file manager after installing) and fill in the `PROTECTED_APPS` variable with space-separated package names:

```sh
PROTECTED_APPS="com.tencent.mm com.whatsapp"
```

Each listed package will be added to the Doze whitelist and get an `oom_score_adj` boost, making it much harder for the system to kill.

### Uninstalling

Remove the module from the Magisk app (**Modules** → tap **Remove** → reboot). This reverts all `settings put` values on next boot and removes the `system.prop` overrides.

### Disclaimer

This module modifies system-level process management and power-saving behavior. It is provided **as-is**, with no warranty. Battery life impact will vary by device, apps installed, and usage pattern. Use at your own risk.

### Author

**Telegram: [@VestronVulture](https://t.me/VestronVulture)**

---

<a id="русский"></a>
## 🇷🇺 Русский

### Что делает модуль

Flyme 11 (Meizu/Flyme OS) известна довольно агрессивной выгрузкой фоновых приложений, из-за чего часто пропадают уведомления в мессенджерах, музыкальных плеерах и трекерах. **Flyme Battery Keeper** смягчает эти лимиты до сбалансированного состояния — заметно мягче, чем в стоковой Flyme, но без крайностей, которые сажают батарею.

Это **не** модуль в стиле «все приложения живут вечно». Каждое значение подобрано так, чтобы получить *большой* прирост стабильности работы приложений за *небольшую* цену в расходе батареи, без перегибов, которые вызывают проблемы с нагревом/CPU или полностью съедают заряд.

### Проверено на

- **Meizu 21**, Flyme 11
- Qualcomm Snapdragon 8 Gen 3

На других Qualcomm-устройствах с Flyme модуль может работать, но настройка выполнена именно под это поколение чипсета.

### Что именно меняется

| Область | Изменение |
|---|---|
| Лимит фоновых приложений Qualcomm | Повышен до **40** процессов |
| Пороги кэша/trim Qualcomm | Сбалансированы (`empty_app_percent=85`, `trim_cache_percent=85` и т. д.) |
| LMKD (Low Memory Killer) | Слегка ослаблены уровни давления (`800/700/600`) — не отключён |
| Максимум кэшированных процессов | Повышен до **150** (в стоковой Flyme — 32) |
| Phantom process killer | Оставлен **включённым** (смягчён вышеописанными настройками, а не отключён) |
| Фоновое сканирование Wi‑Fi / BLE | Отключено — это реальный, но незаметный источник разряда батареи, почти не связанный с «выгрузкой приложений» |
| Адаптивная батарея | **Не тронута** — оставлена включённой, в отличие от прошлых версий модуля |
| Защищённые приложения | Опционально: добавление в белый список Doze + повышение `oom_score_adj` для указанных пакетов |

### Почему выбраны именно такие значения

- **`bg_apps_limit=40`** — заметно больше стокового лимита Flyme, но не «пусть работает всё и всегда», что само по себе сажает батарею из-за постоянного давления на память и фоновой нагрузки CPU.
- **Уровни LMKD (`800/700/600`)** вместо распространённого трюка «поставить всем 1001» — если выставить всем 1001, система вообще перестаёт подчищать память, из-за чего парадоксально чаще случаются тяжёлые циклы полной сборки мусора и подкачки — и это ещё сильнее садит батарею. Данный модуль даёт системе «дышать».
- **`max_cached_processes=150`** вместо экстремальных 256 — нужные вам приложения остаются в памяти, но RAM/CPU не раздувается до состояния, когда система сама начинает тормозить.
- **Отключение постоянного сканирования Wi‑Fi/BLE** — непрерывное сканирование для геолокации в фоне — один из самых заметных и при этом самых незаметных источников разряда батареи, и он никак не связан с лимитами фоновых приложений.
- **Адаптивная батарея оставлена включённой** — в отличие от предыдущей версии модуля, эта сборка не отключает встроенную оптимизацию заряда/использования Android.

### Требования

- Устройство с root-доступом и установленным **Magisk** (или совместимым форком, например KernelSU/APatch с поддержкой модулей Magisk)
- Flyme 11 на устройстве с Qualcomm Snapdragon 8 Gen 3 (на других конфигурациях — на свой риск)

### Установка

1. Скачайте ZIP-архив модуля со страницы [Releases](../../releases) (или сделайте архив из этого репозитория).
2. Откройте **Magisk** → **Модули** → **Установить из хранилища**.
3. Выберите ZIP-файл и подтвердите установку.
4. **Перезагрузите** устройство.

### Настройка — защита конкретных приложений

По умолчанию ни одно приложение принудительно не защищается — модуль просто смягчает системные лимиты. Если хотите сохранить работу конкретных приложений (например, мессенджера) более надёжно, отредактируйте `service.sh` до прошивки модуля (или через root-файловый менеджер после установки) и укажите пакеты через пробел в переменной `PROTECTED_APPS`:

```sh
PROTECTED_APPS="com.tencent.mm com.whatsapp"
```

Каждый указанный пакет будет добавлен в белый список Doze и получит повышение `oom_score_adj`, из-за чего системе будет гораздо сложнее его выгрузить.

### Удаление

Удалите модуль через приложение Magisk (**Модули** → **Удалить** → перезагрузка). После этого все значения `settings put` откатятся при следующей загрузке, а изменения из `system.prop` перестанут применяться.

### Дисклеймер

Модуль изменяет системное управление процессами и энергосбережением. Он предоставляется **как есть**, без каких-либо гарантий. Влияние на автономность зависит от конкретного устройства, установленных приложений и характера использования. Используйте на свой страх и риск.

### Автор

**Telegram: [@VestronVulture](https://t.me/VestronVulture)**
