<template>
  <div id="dragAndZoomId" :style="{
    minWidth: dragAndZoomMinW + 'px',
    minHeight: dragAndZoomMinH + 'px'
  }">
    <div id="dragAndZoomContent">
      <slot></slot>
    </div>
    <div id="topBorder"></div>
    <div id="rightBorder"></div>
    <div id="bottomBorder"></div>
    <div id="leftBorder"></div>
    <div id="topRightAngle"></div>
    <div id="topLeftAngle"></div>
    <div id="bottomRightAngle"></div>
    <div id="bottomLeftAngle"></div>
  </div>
</template>

<script>
  export default {
    props:{
      dragAndZoomMinW:{
        type:Number,
        default:400
      },
      dragAndZoomMinH:{
        type:Number,
        default:300
      }
    },
    data() {
      return {  }
    },
    mounted() {
      this.dragDropMove("dragAndZoomId");
      this.zoomWideHigh("dragAndZoomId","topBorder");
      this.zoomWideHigh("dragAndZoomId","rightBorder");
      this.zoomWideHigh("dragAndZoomId","bottomBorder");
      this.zoomWideHigh("dragAndZoomId","leftBorder");
      this.zoomWideHigh("dragAndZoomId","topRightAngle");
      this.zoomWideHigh("dragAndZoomId","topLeftAngle");
      this.zoomWideHigh("dragAndZoomId","bottomRightAngle");
      this.zoomWideHigh("dragAndZoomId","bottomLeftAngle");
    },
    methods: {

      // 拖拽移动
      dragDropMove(id) {
        //获取元素
        var doc = document.getElementById(id);
        var w = 0;
        var h = 0;
        var l = 0;
        var t = 0;
        var x = 0;
        var y = 0;
        var isDown = false;

        //鼠标按下事件
        doc.onmousedown = registerEvent;

        //鼠标抬起事件
        doc.onmouseup = cancellationEvent;

        // 注册事件
        function registerEvent(e) {
          if(e.stopPropagation)
            e.stopPropagation();
          else e.cancelBubble=true;

          // 获取宽高
          w = doc.offsetWidth;
          h = doc.offsetHeight;

          //计算x坐标和y坐标值
          x = e.clientX;
          y = e.clientY;

          //获取左部和顶部的偏移量
          l = doc.offsetLeft;
          t = doc.offsetTop;

          //开关打开
          isDown = true;
          //鼠标移动
          window.onmousemove = dragDropDisplacement;
        };

        // 注销事件
        function cancellationEvent(e) {
          if(e.stopPropagation)
            e.stopPropagation();
          else e.cancelBubble=true;

          l = 0;
          t = 0;
          x = 0;
          y = 0;
          //开关关闭
          isDown = false;
          window.onmousemove = null;
        };

        // 执行拖拽
        function dragDropDisplacement(e) {
          if(e.stopPropagation)
            e.stopPropagation();
          else e.cancelBubble=true;

          if (isDown == false) {
            return;
          }
          //获取x和y
          var nx = e.clientX;
          var ny = e.clientY;
          //计算移动后的左偏移量和顶部的偏移量
          var nl = nx - (x - l);
          var nt = ny - (y - t);

          doc.style.left = nl + 'px';
          doc.style.top = nt + 'px';
        }
      },

      // 缩放
      zoomWideHigh(id,optId) {
        var _this = this;
        //获取元素
        var doc = document.getElementById(id);
        var optDoc = document.getElementById(optId);
        var w = 0;
        var h = 0;
        var l = 0;
        var t = 0;
        // 记录初始鼠标位置
        var initialNX = null;
        var initialNY = null;
        var isDown = false;

        //鼠标按下事件
        optDoc.onmousedown = registerEvent;

        //鼠标抬起事件
        optDoc.onmouseup = cancellationEvent;

        // 注册事件
        function registerEvent(e) {
          if(e.stopPropagation)
            e.stopPropagation();
          else e.cancelBubble=true;

          // 获取宽高
          w = doc.offsetWidth;
          h = doc.offsetHeight;

          //获取左部和顶部的偏移量
          l = doc.offsetLeft;
          t = doc.offsetTop;

          //开关打开
          isDown = true;
          //鼠标移动
          window.onmousemove = modifyWideHigh;
        };

        // 注销事件
        function cancellationEvent(e) {
          if(e.stopPropagation)
            e.stopPropagation();
          else e.cancelBubble=true;

          l = 0;
          t = 0;
          w = 0;
          h = 0;
          initialNX = null;
          initialNY = null;
          //开关关闭
          isDown = false;
          window.onmousemove = null;
        };

        // 执行缩放
        function modifyWideHigh(e) {
          if(e.stopPropagation)
            e.stopPropagation();
          else e.cancelBubble=true;

          if (isDown == false) {
            return;
          }
          if(initialNX==null){
            initialNX = e.clientX;
          }
          if(initialNY==null){
            initialNY = e.clientY;
          }

          //计算移动后的左偏移量和顶部的偏移量
          var nl = (e.clientX - initialNX);
          var nt = (e.clientY - initialNY);

          // 拖拽位移
          if(false){
            doc.style.left = nl + 'px';
            doc.style.top = nt + 'px';
          }

          // 左上
          if(optId == "topLeftAngle"){
            // 宽度 = (x偏移量*-1)+原来宽度;
            doc.style.width = _this.setWidth( ((nl*-1) + w) , function(){
              // x定位 = 偏移量+左边距;
              doc.style.left = (nl + l) + 'px';
            }) + 'px';
            // 高度 = (y偏移量*-1)+原来高度;
            doc.style.height = _this.setHeight( ((nt*-1) + h) , function(){
              // y定位 = 偏移量+上边距;
              doc.style.top = (nt + t) + 'px';
            }) + 'px';
          }else
          // 左下
          if(optId == "bottomLeftAngle"){
            // 宽度 = (x偏移量*-1)+原来宽度;
            doc.style.width = _this.setWidth( ((nl*-1) + w) , function(){
              // x定位 = 偏移量+左边距;
              doc.style.left = (nl + l) + 'px';
            }) + 'px';
            // 高度 = y偏移量+原来高度;
            doc.style.height = _this.setHeight( (nt + h) ) + 'px';
          }else
          // 右上
          if(optId == "topRightAngle"){
            // 宽度 = x偏移量+原来宽度;
            doc.style.width = _this.setWidth( (nl + w) ) + 'px';
            // 高度 = (y偏移量*-1)+原来高度;
            doc.style.height = _this.setHeight( ((nt*-1) + h) , function(){
              // y定位 = 偏移量+上边距;
              doc.style.top = (nt + t) + 'px';
            }) + 'px';
          }else
          // 右下
          if(optId == "bottomRightAngle"){
            // 宽度 = x偏移量+原来宽度;
            doc.style.width = _this.setWidth( (nl + w) ) + 'px';
            // 高度 = y偏移量+原来高度;
            doc.style.height = _this.setHeight( (nt + h) ) + 'px';
          }else
          // 上边线
          if(optId == "topBorder"){
            // 高度 = (y偏移量*-1)+原来高度;
            doc.style.height = _this.setHeight( ((nt*-1) + h) , function(){
              // y定位 = 偏移量+上边距;
              doc.style.top = (nt + t) + 'px';
            }) + 'px';
          }else
          // 下边线
          if(optId == "bottomBorder"){
            // 高度 = y偏移量+原来高度;
            doc.style.height = _this.setHeight( (nt + h) ) + 'px';
          }else
          // 左边线
          if(optId == "leftBorder"){
            // 宽度 = (x偏移量*-1)+原来宽度;
            doc.style.width = _this.setWidth( ((nl*-1) + w) , function(){
              // x定位 = 偏移量+左边距;
              doc.style.left = (nl + l) + 'px';
            }) + 'px';
          }else
          // 右边线
          if(optId == "rightBorder"){
            // 宽度 = x偏移量+原来宽度;
            doc.style.width = _this.setWidth( (nl + w) ) + 'px';
          }
        }
      },

      // 设置最小宽度
      setWidth(w , callback){
        if(w > this.dragAndZoomMinW){
          callback && callback();
          return w;
        }else{
          return this.dragAndZoomMinW;
        }
      },

      // 设置最小高度
      setHeight(h , callback){
        if(h > this.dragAndZoomMinH){
          callback && callback();
          return h;
        }else{
          return this.dragAndZoomMinH;
        }
      },

    }
  }
</script>

<style scoped>
#dragAndZoomId {
box-sizing: border-box;
position: fixed;
top: 40%;
left: 40%;
z-index: 10000;
background: #FFFFFF;
cursor: move;
-moz-box-shadow:0px 2px 20px #a0a0a0; 
-webkit-box-shadow:0px 2px 20px #a0a0a0; 
box-shadow:0px 2px 20px #a0a0a0; 
}
#topBorder {
width: 100%;
height: 6px;
position: absolute;
top: -3px;
left: 0;
right: 0;
z-index: 10001;
cursor: s-resize;
}
#rightBorder {
width: 6px;
height: 100%;
position: absolute;
top: 0;
right: -3px;
bottom: 0;
z-index: 10001;
cursor: w-resize;
}
#bottomBorder {
width: 100%;
height: 6px;
position: absolute;
bottom: -3px;
left: 0;
right: 0;
z-index: 10001;
cursor: s-resize;
}
#leftBorder {
width: 6px;
height: 100%;
position: absolute;
top: 0;
left: -3px;
bottom: 0;
z-index: 10001;
cursor: w-resize;
}
#topRightAngle {
width: 6px;
height: 6px;
position: absolute;
top: -3px;
right: -3px;
z-index: 10002;
cursor: ne-resize;
}
#topLeftAngle {
width: 6px;
height: 6px;
position: absolute;
top: -3px;
left: -3px;
z-index: 10002;
cursor: se-resize;
}
#bottomRightAngle {
width: 6px;
height: 6px;
position: absolute;
bottom: -3px;
right: -3px;
z-index: 10002;
cursor: se-resize;
}
#bottomLeftAngle {
width: 6px;
height: 6px;
position: absolute;
bottom: -3px;
left: -3px;
z-index: 10002;
cursor: ne-resize;
}
#dragAndZoomContent{
width: 100%;
height: 100%;
}
</style>
