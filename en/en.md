<p align="right">
English description | <a href="/ru/ru.md">Описание на русском</a>
</p>


# File structure

### Here I will describe, which rules I use for `File hierarchy` in my projects
### I will use default react with vite and typescript build 

Firstly, look at files that I create at the same level with `/src` and  `/public` 
There are `.env`, `env-example.md`(for name examples of enviroment variables) and `Dockerfile`. Others are created on this level by default build of vite. 

```
├── public/                 # contain only tab icon
    └── icon.svg            # tab icon
├── src/                    # src folder with all project components
├── .env                    # file with environments 
├── Dockerfile              # Dockerfile for build             
├── env-example.md          # enviroment example file
```

# `/src`

`/src` directory contain a lot of directories and by default `App.tsx` and `main.tsx` that are on the same level

```
├── public/                 # contain only tab icon
    └── icon.svg            # tab icon
├── src/                    # src folder with all project components
    ├── assets              # images, icons and fonts
    ├── axios               # API lib settings 
    ├── components          # components
    ├── consts              # consts 
    ├── context             # context hooks
    ├── enums               # enums 
    ├── helpers             # helper functions
    ├── hooks               # usefull hooks
    ├── layout              # layout
    ├── models              # models/types
    ├── regexp              # regular expressions
    ├── routes              # protected routes components
    ├── service             # API services
    ├── styles              # css files
    ├── utils               # usefull js functions
    ├── App.tsx             # App file
    └── main.tsx            # initializing file for context, router etc.
├── .env                    # file with environments 
├── Dockerfile              # Dockerfile for deploy 
└── env-example.md          # enviroment example file
```

## `/src/App.tsx`
used as container for `routes`

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
used as container for `providers or any methods that must be called as soon as possible`

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

`/src/assets` are created for `Fonts`, `Icons` and `Images`

```
├── src/                    # src folder with all project components
    ├── assets              # images, icons and fonts
        ├── fonts           # fonts
        ├── icons           # vector icons
        └── images          # images (.png, .jpg etc.)
```

### `/src/assets/fonts` 
have this structure for fonts that I save localy. 

#### `Rule #1` Easy describe that way `1 font - 1 directory`

```
├── src/                        # src folder with all project components
    ├── assets                  # images, icons and fonts
        ├── fonts               # fonts
            ├── Inter           # Inter font
                └── Inter.ttf   # Inter font variable
            └── Roboto          # Roboto font
                └── Roboto.ttf  # Roboto font variable
        ├── icons               # vector icons
        └── images              # images (.png, .jpg etc.)
```

### `/src/assets/icons`

will contain icons that grouped up by directories.
This groups can be created for component (like `@/datepicker`), for special group of components (like `@/nav` and `@/systemMsg`) and group for shared icons, I use at the hole project (like `@/base`)

And the most important thing with icons, that I add `-icon` to all names.This is cosmetic habbit, that help me detect icons in search results

#### `Rule #2` `/src/assets/icons` must contain only vector icons 
  
```
├── src/                        # src folder with all project components
    ├── assets                  # images, icons and fonts
        ├── fonts               # fonts
        ├── icons               # vector icons
            ├── base            # base icon (edit, add etc.)
            ├── nav             # navigation icons
            ├── datepicker      # iconc for datepicker
            └── systemMsg       # icons for system messages
        └── images              # images (.png, .jpg etc.)
```

### `/src/assets/images`
use the same rules as `/src/assets/icons` but there is no cosmetic suffix names.
Here important is that `@/images` contain files only with image file format (`.png`, `.jpg` etc.)

```
├── src/                        # src folder with all project components
    ├── assets                  # images, icons and fonts
        ├── fonts               # fonts
        ├── icons               # vector icons
        └── images              # images (.png, .jpg etc.)
            └── background      # background images           
```

## `/src/axios`
here I save files for API default setting for `axios` lib
Any other API lib or any default API rules will be created in same directory and `Directory name depends on lib, that used for API requests`

```
├── src/                        # src folder with all project components
    ├── axios                   # API settings
        ├── axios.d.ts          # global types for axios
        └── axios.ts            # file with settings
```

## `/src/components`
at this level you can see base components directories

#### `Rule #3` When I work with base css, I always create a file with the same name of component in lowercase in component directory

```
├── src/                        # src folder with all project components
    ├── components              # API settings
        ├── Header              # Header
            ├── header.css      # styles for header
            └── Header.tsx      # Header component
        ├── NotFound            # 404 comp
        ├── Example             # example component
        └── UI                  # folder with reusable components
```

### `src/components/UI`
directory that used for many times reusable components (like `buttons`, `inputs`, `selects` etc.)

#### `Rule #4` `/src/components/UI` contain only reusable components like list items, inputs, selects, buttons, titles. `/src/components` contain a huge components that are contructed from small ones with additional logic 

```
├── src/                        # src folder with all project components
    ├── components              # API settings
        ├── Header              # Header
        ├── NotFound            # 404 comp
        ├── Example             # example component
        └── UI                  # folder with reusable components
            ├── Loader          # Loader
            ├── Title           # Title h tag
            ├── Button          # Button 
            ├── Input           # Input
            └── Select          # Select
```

## `/src/consts`
here I save all objects, arrays that will be used by components

```
├── src/                        # src folder with all project components
    ├── consts                  # file for constans
        ├── NavLinks            # navigation links 
            └── NavLinks.tsx    # file with links
        └── SystemObj           # constans for system components
            ├── Icons.tsx       # icons (example below)
            └── SystemMsg.ts    # absoluse constant for system messages
```

(Example of object that save for system messages)

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
contain context hooks.
Also, can be used for list of slices, if you use `RTK`

```
├── src/                        # src folder with all project components
    ├── context                 # context hooks
        ├── useAuth.tsx         # authentification
        ├── useSystemMsg.tsx    # system messages 
        └── useExample.tsx      # example  
```

## `/src/enums`
contain enums for constans names.

```
├── src/                        # src folder with all project components
    ├── enums                   # enums
        └── localStorage.ts     # localStorage variable names (example below)
```

(Example of name that this directory used for)

```typescript
export const LOCAL_STORAGE = {
    ACCESS_TOKEN: "access_token"
} as const
```

## `/src/helpers`
contain any helper function that will used for API requests or HOCs

```
├── src/                        # src folder with all project components
    ├── helpers                 # helper functions
        └── ErrorHandler.ts     # handler for API responces
```

## `/src/hooks`
hooks that contain default needed logic. For example, callback when I click outside triggered component

```
├── src/                        # src folder with all project components
    ├── hooks                   # usefull hooks
        └── useOutlineClick.ts  # callback when click outside triggered component
```

## `/src/layout`
created for `Layout` component that used like container for components,that must be at the highest level in html structure
For example, `modals`, `side menus`, `system messages` etc.

```
├── src/                        # src folder with all project components
    ├── layout                  # layout
        ├── layout.css          # styles for layout
        └── Layout.tsx          # component
```

(Example)

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
contain directories with .ts files for `types`, `interfaces`
Also, must be grouped up by additional directories (like `@/models/API`)

#### `Rule #5` All types and interafaces must be saved in `@/models`

```
├── src/                        # src folder with all project components
    ├── models                  # models/types
        ├── API                 # API types
            └── AuthAPI.ts      # Authentification API types
        ├── Button              # Button types
        ├── Input               # Input types
        ├── Datepicker          # Datepicker types
        ├── SystemElems         # System elements types
        └── Select              # Select types
```

## `/src/regexp`
there is regular expression constans
```
├── src/                        # src folder with all project components
    ├── regexp                  # regular expression variables
        └── URLregexp.ts        # URL regular expression
```

## `/src/routes`
contain file that used for route logic (like protected routes)

```
├── src/                        # src folder with all project components
    ├── routes                  # protected routes components
        └── ProtectedRoutes.tsx # component
```

(Example)

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
used for API requests functions

```
├── src/                        # src folder with all project components
    ├── service                 # API requests
        ├── AuthService.ts      # Authentifications requests
        └── UserService.ts      # user requests
```

(Example)
(when I work with `axios`, I always write functions in service, but `Class` aren't forbidden)

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
used for css files that must be reachable in hole project (`color and font variables`, animations etc.)
`@/styles/index.css` collect all css files and is exported for `main.tsx`

```
├── src/                        # src folder with all project components
    ├── styles                  # css files
        ├── color.css           # color variables
        ├── font-family.css     # fonts variables
        └── index.css           # main css exported file
```

## `/src/utils`
directory with base `js/ts` functions

```
├── src/                        # src folder with all project components
    ├── utils                   # js/ts functions
        └── getDay.ts           # getDay function
```

