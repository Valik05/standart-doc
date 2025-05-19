# Standart documentation

### Here I will describe, which rules I use for `File hierarchy` in my projects
### I will use default react with vite and typescript build 

Firstly, look at files that I create at the same level with `/src` and  `/public` 
There are `.env`,`env-example.md`(for name examples of enviroment variables) and `Dockerfile`. Others are created on this level by default build of vite. 

![alt text](/images/initial.png)

# `/public`

In this directory I save only icon,that will be used as a tab icon

![alt text](/images/public.png)

# `/src`

`/src` directory contain a lot of directories and by default `App.tsx` and `main.tsx` that are on the same level

![alt text](/images/src.png)

## `/src/App.tsx`
used as container for `routes`

![alt text](/images/src/App.tsx.png)

## `/src/main.tsx`
used as container for `providers or any methods that must be called as soon as possible`

![alt text](/images/src/main.tsx.png)

## `/src/assets/`

`/src/assets` are created for `Fonts`, `Icons` and `Images`

![alt text](/images/src/assets/assets.png)

### `/src/assets/fonts` 
have this structure for fonts that I save localy. 

#### `Rule #1` Easy describe that way `1 font - 1 directory`

![alt text](/images/src/assets/fonts/fonts.png)

### `/src/assets/icons`

will contain icons that grouped up by directories.
This groups can be created for component (like `@/datepicker`), for special group of components (like `@/nav` and `@/systemMsg`) and group for shared icons, I use at the hole project (like `@/base`)

And the most important thing with icons, that I add `-icon` to all names.This is cosmetic habbit, that help me detect icons in search results

#### `Rule #2` `/src/assets/icons` must contain only vector icons 
  
![alt text](/images/src/assets/icons/icons.png)

### `/src/assets/images`
use the same rules as `/src/assets/icons` but there is no cosmetic suffix names.
Here important is that `@/images` contain files only with image file format (`.png`, `.jpg` etc.)

![alt text](/images/src/assets/images/images.png)

## `/src/axios`
here I save files for API default setting for `axios` lib
Any other API lib or any default API rules will be created in same directory and `Directory name depends on lib, that used for API requests`

![alt text](/images/src/axios/axios.png)

## `/src/components`
at this level you can see base components directories

#### `Rule #3` When I work with base css, I always create a file with the same name of component in lowercase in component directory

![alt text](/images/src/components/components.png)

### `src/components/UI`
directory that used for many times reusable components (like `buttons`, `inputs`, `selects` etc.)

#### `Rule #4` `/src/components/UI` contain only reusable components like list items, inputs, selects, buttons, titles. `/src/components` contain a huge components that are contructed from small ones with additional logic 

![alt text](/images/src/components/UI/UI.png)

## `/src/consts`
here I save all objects, arrays that will be used by components

![alt text](/images/src/consts/consts.png)

(Example of object that save for system messages)

![alt text](/images/src/consts/example.png)

## `/src/context`
contain context hooks.
Also, can be used for list of slices, if you use `RTK`

![alt text](/images/src/context/context.png)

## `/src/enums`
contain enums for constans names.

![alt text](/images/src/enums/enums.png)

(Example of name that this directory used for)

![alt text](/images/src/enums/example.png)

## `/src/helpers`
contain any helper function that will used for API requests or HOCs

![alt text](/images/src/helpers/helpers.png)

## `/src/hooks`
hooks that contain default needed logic. For example, callback when I click outside triggered component

![alt text](/images/src/hooks/hooks.png)

## `/src/layout`
created for `Layout` component that used like container for components,that must be at the highest level in html structure
For example, `modals`, `side menus`, `system messages` etc.

![alt text](/images/src/layout/layout.png)

(Example)

![alt text](/images/src/layout/example.png)

## `/src/models`
contain directories with .ts files for `types`, `interfaces`
Also, must be grouped up by additional directories (like `@/models/API`)

#### `Rule #5` All types and interafaces must be saved in `@/models`

![alt text](/images/src/models/models.png)

## `/src/regexp`
there is regular expression constans

![alt text](/images/src/regexp/regexp.png)

## `/src/routes`
contain file that used for route logic (like protected routes)

![alt text](/images/src/routes/routes.png)

(Example)

![alt text](/images/src/routes/example.png)

## `/src/service`
used for API requests functions

![alt text](/images/src/service/service.png)

(Example)
(when I work with `axios`, I always write functions in service, but `Class` aren't forbidden)

![alt text](/images/src/service/example.png)

## `/src/styles`
used for css files that must be reachable in hole project (`color and font variables`, animations etc.)
`@/styles/index.css` collect all css files and is exported for `main.tsx`

![alt text](/images/src/styles/styles.png)

## `/src/utils`
directory with base `js/ts` functions

![alt text](/images/src/utils/utils.png)

