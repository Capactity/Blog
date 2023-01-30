```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta
      content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover"
      name="viewport"
    />
    <title>hash路由</title>
    <style>
      * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
      }
      html,
      body {
        height: 100%;
      }
      #content {
        height: calc(100vh - 50px);
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 3em;
      }
      #nav {
        height: 50px;
        position: fixed;
        left: 0;
        bottom: 0;
        width: 100%;
        display: flex;
      }
      #nav a {
        width: 25%;
        display: flex;
        justify-content: center;
        align-items: center;
        border: 1px solid black;
      }
      #nav a:not(:last-of-type) {
        border-right: none;
      }
    </style>
  </head>
  <body>
    <main id="content"></main>
    <nav id="nav">
      <a href="#/">首页</a>
      <a href="#/list">列表</a>
      <a href="#/detail">详情</a>
      <a href="#/user">我的</a>
    </nav>
  </body>
  <script>
    class VueRouter {
      constructor(routes = []) {
        this.routes = routes; // 路由映射
        this.currentHash = ""; // 当前的hash
        this.refresh = this.refresh.bind(this);
        window.addEventListener("load", this.refresh, false);
        window.addEventListener("hashchange", this.refresh, false);
      }

      getUrlPath(url) {
        // 获取hash
        return url.indexOf("#") >= 0 ? url.slice(url.indexOf("#") + 1) : "/";
      }

      refresh(event) {
        // URL hash发生改变的时候，拿到当前的hash
        let newHash = "",
          oldHash = null;
        if (event.newURL) {
          oldHash = this.getUrlPath(event.oldURL || "");
          newHash = this.getUrlPath(event.newURL || "");
        } else {
          newHash = this.getUrlPath(window.location.hash);
        }
        this.currentHash = newHash;
        this.matchComponent();
      }

      matchComponent() {
        let curRoute = this.routes.find(
          (route) => route.path === this.currentHash
        );
        if (!curRoute) {
          // 当前URL中的hash不存在的时候，默认取第一个，可能会有各种情况，取决于业务逻辑
          curRoute = this.routes.find((route) => route.path === "/");
        }
        const { component } = curRoute;
        document.querySelector("#content").innerHTML = component;
      }
    }

    const router = new VueRouter([
      {
        path: "/",
        name: "home",
        component: "<div>首页内容</div>",
      },
      {
        path: "/list",
        name: "list",
        component: "<div>列表</div>",
      },
      {
        path: "/detail",
        name: "detail",
        component: "<div>详情页</div>",
      },
      {
        path: "/user",
        name: "user",
        component: "<div>我的内容</div>",
      },
    ]);
  </script>
</html>
```
