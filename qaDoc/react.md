## 项目创建

### yarn

````
yarn create react-app my-app
````

### npm

```
npx create-react-app my-app
```

### 没有 webpack.config.js

```
执行后有 config 文件夹
npm run eject
```

## 组件

### 组件类型

#### 函数组件

```react
function Home(props) {
    // 依赖设置为空，表示为只有第一次加载时执行，避免重复请求
    // react@v18 app.js 中使用 <React.StrictMode> 严格模式下， useEffect 会执行两遍，但生产环境不影响
    useEffect(() => {
      console.log('请求 API');
    },[])
    return (
        <div>
        	Home
        </div>
    )
}
export default Home;
```

#### 类组件

```react
class Home extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

### 组件用法

#### 变量

```react
const [navActive, setNavActive] = useState(0);
```

#### 参数

```react
function Home(props) {
    return (
        <div>
        	{props.name}
        </div>
    )
}
export default Home;
```

### 第三方组件

#### 字节跳动图标组件

1. 安装 : yarn add @icon-park/react
2. 引入 css：import '@icon-park/react/styles/index.css';
3. 引入图标：import {Home} from '@icon-park/react';
4. 完整文档：https://github.com/bytedance/IconPark/blob/master/packages/react/README.md
5. 图标库：https://iconpark.oceanengine.com/official

#### semi 组件库修改主题

1. yarn add -D @douyinfe/semi-webpack-plugin

2. npm i -s @semi-bot/semi-theme-pxaisms

3. 在 webpack.config.js 里配置

   ```react
   const SemiWebpackPlugin = require('@douyinfe/semi-webpack-plugin').default;
    
   plugins: [
     // 注意，请将Semi的Plugin在最前置入
     new SemiWebpackPlugin({
         theme: '@ies/semi-theme-semi-design-system-demo'
          /* ...options */
     }), 
   ]
   ```

## 路由(v6)

### 安装

```
yarn add react-router-dom
npm install -save react-router-dom
```

### 代码

```react
// 路由配置
<Router>
  <Routes>
    <Route path="/" element={<Home/>}/>
    <Route path="/user/mine" element={<Mine/>}/>
  </Routes>
</Router>

const navigate = useNavigate();
navigate(page); // 路由跳转
const location = useLocation();
const cpathname = location.pathname; // 当前路径名
```
