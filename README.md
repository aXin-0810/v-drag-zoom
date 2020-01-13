v-drag-zoom
===

v-drag-zoom组件描述

全局浮动模块拖拽移动与缩放


```javascript

  // 在vue项目根目录执行命令
  npm install v-drag-zoom

  // 在main.js文件下引入
  import vDragZoom from 'v-drag-zoom'
  // 注册使用
  Vue.use(vDragZoom)

  // 全局使用
  <v-drag-zoom 
	:dragAndZoomMinW="400"				//最小宽度
	:dragAndZoomMinH="300">				//最小高度
    <div>插槽区域</div>
  </v-drag-zoom>

```



更新日志
=======

  * v1.0.2

许可证
=======

Code copyright 2012-2020 axin_0810
