<p align="right">
<a href="/en/en.md">English description</a> | Описание на русском
</p>


# Структура файлов

### Здесь я опишу правила, которые использую для `иерархии файлов` в своих проектах
### Используется стандартный стек React с Vite и TypeScript

Сначала рассмотрим файлы, которые я создаю на одном уровне с папками `/src` и `/public`.
Это `.env`, `env-example.md` (пример названий переменных окружения) и `Dockerfile`. Остальные файлы создаются по умолчанию при сборке Vite.

```
├── public/                 # содержит только иконку вкладки
    └── icon.svg            # иконка вкладки
├── src/                    # папка src со всеми компонентами проекта
├── .env                    # файл с переменными окружения
├── Dockerfile              # Dockerfile для сборки             
├── env-example.md          # пример файла переменных окружения
```

# `/src`

Папка `/src` содержит множество директорий, а также по умолчанию `App.tsx` и `main.tsx`, которые находятся на одном уровне.

```
├── public/                 # папка для иконки вкладки
    └── icon.svg            # иконка вкладки
├── src/                    # папка со всем проектом
    ├── assets              # изображения, иконки и шрифты
    ├── axios               # настройки API библиотеки
    ├── components          # компоненты
    ├── consts              # константы
    ├── context             # контексты
    ├── enums               # перечисления
    ├── helpers             # вспомогательные функции
    ├── hooks               # полезные хуки
    ├── layout              # компоненты макета
    ├── models              # модели/типы
    ├── regexp              # регулярные выражения
    ├── routes              # компоненты защищенных маршрутов
    ├── service             # API сервисы
    ├── styles              # CSS файлы
    ├── utils               # полезные функции
    ├── App.tsx             # основной компонент приложения
    └── main.tsx            # инициализация контекста, роутера и пр.
├── .env                    # файл переменных окружения
├── Dockerfile              # инструкции для билда контейнера
└── env-example.md          # пример названий переменных окружения
```

## `/src/App.tsx`
Компонент-контейнер для `routes`.

```typescript
import Layout from './layout/Layout';
import NotFound from './components/NotFound/NotFound';
import ProtectedRoutes from './routes/ProtectedRoutes';
import Advertisement from './components/Advertisement/Advertisement';
import ReportCreater from './components/ReportCreater/ReportCreater';
import ReportHistory from './components/ReportHistory/ReportHistory';
import { useAuth } from './context/useAuth';
import { Route, Routes } from 'react-router-dom';

function App() {
  const { isAuth } = useAuth();
  return (
    <Routes>
      <Route path='/' element={<Layout />}>
        <Route index element={<ReportCreater />} />
        <Route element={<ProtectedRoutes isLoggenIn={isAuth} redirectPath='/' />}>
          <Route path='/report/:report_id/edit' element={<ReportCreater mode="edit" />} />
          <Route path='/report-history' element={<ReportHistory />} />
          <Route path='/advertisement' element={<Advertisement />} />
        </Route>
        <Route path='*' element={<NotFound />} />
      </Route>
    </Routes>
  )
}

export default App
```

## `/src/main.tsx`
Используется как контейнер для `провайдеров или любых методов, которые должны быть вызваны как можно раньше`.

```typescript
import './styles/index.css';
import App from './App.tsx';
import WebApp from '@twa-dev/sdk';
import { createRoot } from 'react-dom/client';
import { BrowserRouter } from 'react-router-dom';
import { SdkProvider } from './context/useSdk.tsx';
import { AuthProvider } from './context/useAuth.tsx';
import { UserProvider } from './context/useUser.tsx';
import { ReportProvider } from './context/useReport.tsx';
import { FilterProvider } from './context/useFilter.tsx';
import { LoadingProvider } from './context/useLoading.tsx';
import { SideMenuProvider } from './context/useSideMenu.tsx';
import { SystemMsgProvider } from './context/useSystemMsg.tsx';

WebApp.ready();

createRoot(document.getElementById('root')!).render(
  <SdkProvider>
    <BrowserRouter>
      <SystemMsgProvider>
        <AuthProvider>
          <UserProvider>
            <FilterProvider>
              <ReportProvider>
                <SideMenuProvider>
                  <LoadingProvider>
                    <App />
                  </LoadingProvider>
                </SideMenuProvider>
              </ReportProvider>
            </FilterProvider>
          </UserProvider>
        </AuthProvider>
      </SystemMsgProvider>
    </BrowserRouter>
  </SdkProvider>
)
```

## `/src/assets/`

Папка `/src/assets` создана для `шрифтов`, `иконок` и `изображений`.

```
├── src/                    # папка со всем проектом
    ├── assets              # шрифты,изображения и иконкы
        ├── fonts           # шрифты
        ├── icons           # векторные иконки
        └── images          # изображения (.png, .jpg и тд.)
```

### `/src/assets/fonts` 
содержить шрифты, используемые локально 

#### `Правило #1` — `1 шрифт` = `1 папка`

```
├── src/                        # папка со всем проектом
    ├── assets                  # шрифты,изображения и иконкы
        ├── fonts               # шрифты
            ├── Inter           # Inter шрифт
                └── Inter.ttf   # Inter шрифт переменная
            └── Roboto          # Roboto шрифт
                └── Roboto.ttf  # Roboto шрифт переменная
        ├── icons               # векторные иконки
        └── images              # изображения (.png, .jpg и тд.)
```

### `/src/assets/icons`

Содержит векторные иконки, сгруппированные по папкам.
Ети групы могут быть созданы для компонента (как `@/datepicker`), для спецыальной групы комонентов (как `@/nav` and `@/systemMsg`) и просто часто используемые на всём проекте иконки (как `@/base`)

Также важно, что я добавляю суффикс `-icon` к названиям файлов.Ето косметическая привычка, которая помогает отделять иконки от компонентов с результатов в поиске

#### `Правило #2` — в `/src/assets/icons` только векторные иконки 
  
```
├── src/                        # папка со всем проектом
    ├── assets                  # шрифты,изображения и иконкы
        ├── fonts               # шрифты
        ├── icons               # векторные иконки
            ├── base            # базовые иконки (edit, add и тд.)
            ├── nav             # иконки навигации
            ├── datepicker      # для календаря 
            └── systemMsg       # для системные уведомлений
        └── images              # изображения (.png, .jpg и тд.)
```

### `/src/assets/images`
использует такие же правило что и для `/src/assets/icons`,но здесь не применяться косметические суффиксы для названий.
Тут важно то, что `@/images` должен содержать файлы только форматов изображения (`.png`, `.jpg` и тд.)

```
├── src/                        # папка со всем проектом
    ├── assets                  # шрифты,изображения и иконкы
        ├── fonts               # шрифты
        ├── icons               # векторные иконки
        └── images              # изображения (.png, .jpg и тд.)
            └── background      # изображения фона           
```

## `/src/axios`
тут я храню файлы для API стандартных настроек библиотеки `axios`. Другие API библиотеки, либо API правила будут создаваться в етой папке и `название папки будет зависеть от названия библиотеки, которая исполюзуеться`

```
├── src/                        # папка проекта
    ├── axios                   # API настройки
        ├── axios.d.ts          # глобальные типы для axios
        └── axios.ts            # файл с настройками
```

## `/src/components`
на етом уровне вы увидите папку базовых компонентов

#### `Правило #3` — базовые CSS стили размещаются в файле с таким же названием, но строчными буквами внутри папки компонента.

```
├── src/                        # папка проекта
    ├── components              # API настройки
        ├── Header              # хедер
            ├── header.css      # стили для хедера
            └── Header.tsx      # сам компонент
        ├── NotFound            # 404 компонент
        ├── Example             # пример
        └── UI                  # папка для реиспользуемых компонентов
```

### `src/components/UI`
папка для реиспользуемых компонентов, которые будут использоваться в других компонентах (как `кнопки`, `инпуты`, `селекты` и тд.)

#### `Правило #4` — `/src/components/UI` содержит реиспользуемые компоненты (инпут, кнопка и тд.), а `/src/components` — сложные компоненты, собранные из мелких. 

```
├── src/                        # папка проекта
    ├── components              # API настройки
        ├── Header              # хедер
        ├── NotFound            # 404 компонент
        ├── Example             # пример
        └── UI                  # папка для реиспользуемых компонентов
            ├── Loader          # лоудер
            ├── Title           # Title h тэга
            ├── Button          # кнопка 
            ├── Input           # инпут
            └── Select          # селект
```

## `/src/consts`
здесь я храню все объекты, массивы, что буду использоваться для компонентов

```
├── src/                        # папка проекта
    ├── consts                  # папка констант
        ├── NavLinks            # навигационные ссылки
            └── NavLinks.tsx    # файл с ссылками
        └── SystemObj           # константы для системных компонентов
            ├── Icons.tsx       # иконки (пример ниже)
            └── SystemMsg.ts    # абсолютные константы для системных уведомлений
```

(Пример объекта с иконками)

```typescript 
import XIcon from "../../assets/icons/systemMsg/x-icon.svg?react";
import CheckmarkIcon from "../../assets/icons/systemMsg/check-mark-icon.svg?react";
import type { Icons } from "../../models/SystemElems/SystemMsg";

export const SystemMsgIcons: Icons = {
    "success": <CheckmarkIcon />,
    "error": <XIcon />
}
```

## `/src/context`
Содержит хуки контекста
Также, может быть использована для хранения слайсов, если используеться `RTK`

```
├── src/                        # папка проекта
    ├── context                 # контекст хуки
        ├── useAuth.tsx         # авторизация
        ├── useSystemMsg.tsx    # системные уведомления
        └── useExample.tsx      # пример  
```

## `/src/enums`
содержит enums для константных имён

```
├── src/                        # папка проекта
    ├── enums                   # enums
        └── localStorage.ts     # имя для переменных в локальном хранилище (пример ниже)
```

(Пример enum для имени переменной в локальной хранилище)

```typescript
export const LOCAL_STORAGE = {
    ACCESS_TOKEN: "access_token"
} as const
```

## `/src/helpers`
содержит любые функции или обработчики, что используються для API запросов или компонентов вышего порядка

```
├── src/                        # папка проекта
    ├── helpers                 # helper функции
        └── ErrorHandler.ts     # обработчик ответов для API запросов
```

## `/src/hooks`
содержит базовые хуки, логика которых будет реиспользоваться по всему проекту.К примеру, коллбэк если кликаю за пределы функциональной зоны текущего компонента

```
├── src/                        # папка проекта
    ├── hooks                   # полезные хуки
        └── useOutlineClick.ts  # коллбэк из примера
```

## `/src/layout`
Макет верхнего уровня `Layout`, например, для `модальных окон`, `системных сообщений`, `бокового меню`.

```
├── src/                        # папка проекта
    ├── layout                  # layout
        ├── layout.css          # стили для layout
        └── Layout.tsx          # сам компонент
```

(Пример)

```typescript
import './layout.css';
import classNames from "classnames";
import Header from '../components/Header/Header';
import SystemMsg from '../components/SystemMsg/SystemMsg';
import SideMenu from '../components/UI/SideMenu/SideMenu';
import { Outlet } from 'react-router-dom';

const Layout = () => {
    return (
        <article className={classNames('layout-container')}>
            <SideMenu />
            <SystemMsg />
            <Header />
            <Outlet />
        </article>
    )
};

export default Layout;

```

## `/src/models`
Содержит папку с .ts файлами для `типов` и `interface`
Также должно групироваться по допольнытельным директориям (как `@/models/API`)

#### `Правило #5` — все типы и интерфейсы должны храниться в `@/models`

```
├── src/                        # папка проекта
    ├── models                  # типы
        ├── API                 # API типы
            └── AuthAPI.ts      # API авторизации
        ├── Button              # для кнопки
        ├── Input               # для инпута
        ├── Datepicker          # для календаря
        ├── SystemElems         # для системных уведомлений
        └── Select              # для селекта
```

## `/src/regexp`
тут храняться регулярные выражения
```
├── src/                        # папка проекта
    ├── regexp                  # переменные регулярных выражений
        └── URLregexp.ts        # URL регулярное выражение
```

## `/src/routes`
содержит файл,что используеться для управления логикой роутинга (как protected routes)

```
├── src/                        # папка проекта
    ├── routes                  # routes компоненты
        └── ProtectedRoutes.tsx # сам комонент
```

(Пример)

```typescript
import { Navigate, Outlet } from "react-router-dom";
import type { ProtectedRoutesProps } from "../models/Routes/Routes";

const ProtectedRoutes = ({ isLoggenIn, redirectPath, children = false }: ProtectedRoutesProps) => {
    if (!isLoggenIn) return <Navigate to={redirectPath} replace />;
    return children ? children : <Outlet />;
};

export default ProtectedRoutes
```

## `/src/service`
используеться для хранения функций API запросов

```
├── src/                        # папка проекта
    ├── service                 # API запросы
        ├── AuthService.ts      # запросы на авторизацию
        └── UserService.ts      # запросы на пользователя
```

(Пример)
(когда я работаю с `axios`, я всегда пишу функциональные сервиса, но `Class` не запрещенны)

```typescript
import { axiosService } from "../axios/axios";
import { ErrorHandler } from "../helpers/ErrorHandler";
import type { UserSuccessResponce } from "../models/API/UserAPI";

export const getUserAPI = async () => {
    try {
        const { data } = await axiosService.get<UserSuccessResponce>('/users/profile/');
        console.log(data);
        return data;
    } catch (error: unknown) {
        console.log(ErrorHandler(error));
        throw ErrorHandler(error)
    }
};
```

## `/src/styles`
используеться для css файлов, что должны быть доступные по всему проекту (`переменные цветов и шрифтов`, анимации и тд.)
`@/styles/index.css` собирает в себе все css файлы и експортируеться в `main.tsx`

```
├── src/                        # папка проекта
    ├── styles                  # css файлы
        ├── color.css           # переменные цветов
        ├── font-family.css     # переменные шрифтов
        └── index.css           # главный css файл для main.tsx
```

## `/src/utils`
папка с базовыми `js/ts` функциями

```
├── src/                        # папка проекта
    ├── utils                   # js/ts функции
        └── getDay.ts           # getDay функция
```

