## 首页-首页点击详情信息 跳转 但是导航只显示首页

```
 {
        path: '/home',
        name: 'Home',
        redirect: '/home/homeIndex',
        component: PageView,
        hideChildrenInMenu: true, // 强制显示 MenuItem 而不是 SubMenu
        // component: () => import('@/views/dashboard/UnionIndex'),
        meta: { title: '首页', keepAlive: true, icon: 'home', hiddenHeaderContent: false },
        children: [{
          path: '/home/homeDetail',
          name: 'HomeDetail',
          component: () => import('@/views/dashboard/UnionDetail'),
          meta: { title: '资料详情', icon: 'home' }
          // hidden: true
        },
        {
          path: '/home/homeIndex',
          name: 'HomeIndex',
          component: () => import('@/views/dashboard/UnionIndex'),
          meta: { title: '首页', icon: 'home' }
          // hidden: true
        }
        ]
      },
```

### 如果不嵌套不需要面包屑
```
 // 首页
      // {
      //   path: '/home',
      //   name: 'Home',
      //   hideChildrenInMenu: true, // 强制显示 MenuItem 而不是 SubMenu
      //   component: () => import('@/views/dashboard/UnionIndex'),
      //   meta: { title: '首页', keepAlive: true, icon: 'home' },
      //   children: [{
      //     path: '/home/homeDetail',
      //     name: 'HomeDetail',
      //     component: () => import('@/views/dashboard/UnionDetail'),
      //     meta: { title: '资料详情', keepAlive: true, icon: 'home' }
      //     // hidden: true
      //   }]
      // },

      // 详细资料
      // {
      //   path: '/homeDetail',
      //   name: 'HomeDetail',
      //   component: () => import('@/views/dashboard/UnionDetail'),
      //   meta: { title: '资料详情', keepAlive: true, icon: 'home' },
      //   hidden: true
      // },

```