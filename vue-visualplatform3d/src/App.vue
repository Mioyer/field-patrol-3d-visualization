<script>
import { ak } from "./AkContainer";
import Button from './components/Button.vue';
import ControlPad from "./components/ControlPad.vue";
import Header from './components/Header.vue';
import VideoCard from './components/VideoCard.vue';
import TrackingCard from './components/TrackingCard.vue';
import Popup from './components/Popup.vue';
import Switcher from "./components/Switcher.vue";
import "./app/point_processor";
function loadExternalScript(src) {
  return new Promise((resolve, reject) => {
    window.onLoaded = resolve;
    const scriptEl = document.createElement('script');
    scriptEl.type = 'text/javascript';
    scriptEl.src = src + "&callback=onLoaded";
    document.head.appendChild(scriptEl);
    scriptEl.onerror = reject;
  });
}
export default {
  components: {
    Button,
    Header,
    VideoCard,
    TrackingCard,
    Popup,
    ControlPad,
    Switcher
  },
  beforeMount() {
    loadExternalScript('https://api.map.baidu.com/api?v=1.0&&type=webgl&ak=' + ak);
    loadExternalScript('https://api.map.baidu.com/library/TrackAnimation/src/TrackAnimation_min.js');
  },
  mounted() {
    const controlPad = this.$refs.controlPad;
    controlPad.toggle(0);
    this.updateTime();
  },
  data() {
    return {
      currentTime: '',
      webMap: undefined //TODO:导包
    }
  },
  methods: {
    updateTime() {
      setInterval(() => {
        this.currentTime = new Date().toLocaleString('zh-CN');
      }, 1000);
    }
  }
}
</script>

<template>
  <Header ref="header" :webMap="webMap" :mainContent="this.currentTime" subContent="天气" />
  <div id="content">
    <div id="map-content"></div> <!--地图内容--->
    <div id="widget-content"> <!--非地图内容-->
      <div id="left-area">
        <Button id="navigator" pos="left" action="back" icon="src/resources/back.svg"></Button> <!--返回按钮-->
        <Button id="display-control" pos="left" action="control path" icon="src/resources/path.svg"></Button>
        <!--显示元素的控制按钮-->
        <Button id="menu-control" pos="left" action="menu" icon="src/resources/menu.svg"></Button> <!--展开菜单控制-->
        <Button id="timeline-control" pos="left" action="timeline" icon="src/resources/timeline.svg"></Button>
        <!--时间线控制-->
        <ControlPad ref="controlPad" id="control-pad" type="vertical" icon1="src/resources/drone.svg"
          icon2="src/resources/car.svg" alt1="无人机" alt2="小车"></ControlPad>
      </div>
      <VideoCard /> <!--即时视频区域--->
      <TrackingCard /> <!--TODO: vue的自定义元素内容方法-->
      <Popup /> <!--展开菜单--> <!--TODO: vue的显示/隐藏元素的操作方法-->
    </div>
  </div>
  <noscript>该应用程序需要使用Javascript,请允许Javascript运行。</noscript>
</template>
<link rel="stylesheet" href="@/resources/main.css"/>
<style scoped>
#navigator {
  grid-row: 1;
}

#display-control {
  grid-row: 2;
}

#menu-control {
  grid-row: 3;
}

#timeline-control {
  grid-row: 6;
}

#control-pad {
  width: 90px;
  height: 180px;
  position: absolute;
  bottom: -250px;
}

#content {
  width: 100%;
  height: 100%;
  position: fixed;
  /*border: 2px solid red;*/
}

#map-content {
  width: 100%;
  height: 100%;
}

#widget-content {
  width: 100%;
  height: calc(100% - 100px);
  position: fixed;
  top: 100px;
  z-index: 99;
  pointer-events: none;
}

#left-area {
  --button-size: 60px;
  width: var(--button-size);
  height: 60%;
  margin-top: 1%;
  margin-left: 1%;
  padding: 10px;
  display: grid;
  grid-template-columns: 100%;
  grid-template-rows: repeat(8, var(--button-size)) calc(100% - 700px);
  row-gap: 5%;
  justify-items: center;
  align-items: center;
  background: linear-gradient(135deg, var(--day-controller-color-start), var(--day-controller-color-end)),
    linear-gradient(270deg, var(--day-controller-color-end) 20%, var(--day-controller-color-start) 80%);
  border-radius: 20px;
  border: 1px solid rgba(255, 255, 255, 0.3);
  backdrop-filter: blur(50px);
  -webkit-backdrop-filter: blur(50px);
  box-shadow: 0px 8px 32px 0px rgba(0, 0, 0, 0.37);
  pointer-events: all;
}
</style>
<style>
* {
  font-family: "思源黑体 CN", "Helvatica", "Microsoft YaHei UI"
}

:root {
  /*Night mode color definition */
  --night-header-color-start: #04353f90;
  --night-header-color-end: #08155290;
  --night-card-color-start: #04353f90;
  --night-card-color-end: #08155290;
  --night-controller-color-start: #13849b60;
  --night-controller-color-end: #10916160;
  --night-button-color: #3ccdeaaa;
  --night-button-shadow-start: #19a572ee;
  --night-button-shadow-end: #0a5060ee;
  --night-font-color: #ffffff;
  /*Day mode color definition */
  --day-header-color-start: #FFF3DA90;
  --day-header-color-end: #fddcb690;
  --day-card-color-start: #FFF3DA90;
  --day-card-color-end: #fddcb690;
  --day-controller-color-start: #bdaaf060;
  --day-controller-color-end: #ab9aeb60;
  --day-button-color: #ccb6ecaa;
  --day-button-shadow-start: #ecbcf1ee;
  --day-button-shadow-end: #bb6ec0ee;
  --day-font-color: #000000;
}

body {
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.glass-pad {
  background: linear-gradient(135deg, var(--day-controller-color-start), var(--day-controller-color-end)),
    linear-gradient(270deg, var(--day-controller-color-end) 20%, var(--day-controller-color-start) 80%);
  border-radius: 20px;
  border: 1px solid rgba(255, 255, 255, 0.3);
  backdrop-filter: blur(50px);
  -webkit-backdrop-filter: blur(50px);
  box-shadow: 0px 8px 32px 0px rgba(0, 0, 0, 0.37);
  margin-top: 0.5%;
  margin-left: 1%;
}</style>