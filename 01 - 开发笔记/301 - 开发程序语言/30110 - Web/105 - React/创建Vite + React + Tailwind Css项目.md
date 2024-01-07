>技术选型 `react 18`、`react-router-dom`、`redux tookit`、`vite`、`Typescript`、 `tailwind css`、`eslint`

<br/>

## 使用Vite创建项目
```zsh
yarn create vite twocats-admin-web --templcate react-ts
```
<br/>

### 配置 resolve.alias

项目报错<mark class="hltr-red">找不到path对象</mark>需要引入`@types/node`, yarn命令`yarn add @types/node` 

```javascript
// vite.config.ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import path from 'path';

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src')
    }
  }
})
```

如果配置好之后继续报错<mark class="hltr-red">找不到模块</mark>`"@/xxx"` 则需要对Typescript进行配置
```json
// tsconfig.json
{
  "compilerOptions": {
    "paths": {
      "@/*": ["./src/*"],
    }
  }
}
```

### 配置svg图标导入问题
#### 声明文件配置
```
// client.d.ts
declare module '*.svg' {
	import React = require('react');
	export const ReactComponent: React.SFC<react.svgprops<svgsvgelement>>;
	const src: string;
	export default src;
}
```

#### 引入`vite-plugin-svgr`插件
1. 安装插件
```
yarn add vite-plugin-svgr
```

2. 配置插件
```
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import svgr from 'vite-plugin-svgr';

// https://vitejs.dev/config/
export default defineConfig({
	plugins: [react(), svgr()]
})
```
<br/>

## 集成react-router-dom

### 安装react-router-dom
```zsh
yarn add react-router-dom
```

### 配置路由
```typescript
// src/router.tsx
import { lazy, type ReactNode } from 'react'
import { type RouteObject } from 'react-router-dom'

const Login = lazy(() => import('./pages/login'))

export interface PermissionRouteObject extends Omit<RouteObject, 'children'> {
	icon?: ReactNode;
	label?: ReactNode;
	children?: PermissionRouteObject[]
	hidden?: boolean
}

/** 无需认证就可以访问的路由 */
export const noAuthRoutes: PermissionRouteObject[] = [
	{
		path: 'login',
		label: '登录',
		element: <Login />
	}
];

/** 无需鉴权就可以访问的路由 */
export const noAccessRoutes: PermissionRouteObject[] = [];

export const authRoutes: PermissionRouteObject[] = [];

  

const routes: PermissionRouteObject[] = [
	...noAuthRoutes,
	...noAccessRoutes,
	...authRoutes
];

export default routes;
```

### 配置main.tsx页面
```typescript
import React from 'react'
import ReactDOM from 'react-dom/client'
import { Provider } from 'react-redux'
import { BrowserRouter, Routes, Route } from 'react-router-dom'
import { StyleProvider } from '@ant-design/cssinjs'

import './main.css'

import { store } from './store/store'
import Layout from './layout'
import Login from './pages/login'
  

ReactDOM.createRoot(document.getElementById('root') as HTMLElement).render(
<React.StrictMode>
	<Provider store={store}>
		<StyleProvider hashPriority="high">
			<BrowserRouter>
				<Routes>
					<Route path="/" element={<Layout />} />
					<Route path="/login" element={<Login />} />
				</Routes>
			</BrowserRouter>
		</StyleProvider>
	</Provider>
</React.StrictMode>
)
```

### Redirect 替代方案 Navigate
缺省

### Outlet


### useOutletContext 子路由状态共享

### 路由拦截

<br/>

## 集成react-redux
### 安装react-redux
```
yarn add react-redux @reduxjs/toolkit
```

### 配置store和hook
stroe.js文件
```typescript
import { configureStore, ThunkAction, Action } from '@reduxjs/toolkit'
import * as reducer from './slices'

export const store = configureStore({
	reducer,
	middleware: (getDefaultMiddleware) => getDefaultMiddleware({
		serializableCheck: false
	})
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch
export type AppThunk<ReturnType = void> = ThunkAction<
	ReturnType,
	RootState,
	unknown,
	Action<string>
>;
```

hook.js文件
```typescript
import { TypedUseSelectorHook, useDispatch, useSelector } from 'react-redux';
import type { RootState, AppDispatch } from './store';

export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

### 配置需要共享的状态切片
```typescript
// userInfo slice
import { createSlice, type PayloadAction } from '@reduxjs/toolkit'
import { useAppSelector } from '../hook';
import { AppDispatch } from '../store';

import type UserInfo from 'src/interface/UserInfo';

// 为 slice state 定义一个类型
interface UserInfoState {
	value: UserInfo;
}

  
// 使用该类型定义初始 state
const initialState: UserInfoState = {
	value: null
}

const userInfoSlice = createSlice({
	name: 'userInfo',
	initialState,
	reducers: {
		// 使用 PayloadAction 类型声明 `action.payload` 的内容
		update: (state, action: PayloadAction<UserInfo>) => {
			state.value = action.payload
		}
	}
})

export const useUserInfo = () => useAppSelector(state => state.userInfo.value);
export const { update } = userInfoSlice.actions;

export const fetchUserInfo = () => async (dispatch: AppDispatch) => {
	dispatch(update({
		id: '001',
		uid: '0001',
		username: 'wuzx'
	}))
}

export default userInfoSlice.reducer
```

<br/>

## 配置ESlint + Prettier
### 安装eslint
```zsh
# 需要去官网下载最新的包
yarn add -D prettier@^2.5.1 eslint@^8.7.0 @typescript-eslint/parser@^5.0.1 typescript@^4.4.4
```

### 初始化eslint
- 将该插件设置成默认格式
- 在项目中配置`setting.json` 文件，直接使用官方提供的即可
```json
{ 
	"editor.defaultFormatter": "rvest.vs-code-prettier-eslint",//指定默认格式插件 
	 "editor.formatOnSave": true, // 保存自动格式化 
}
```

#### 指定parser
```json
// .eslintrc
{
"parser": "@typescript-eslint/parser", 
"plugins": [], 
"rules": {}
}
```