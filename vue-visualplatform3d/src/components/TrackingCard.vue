<script>
import TrackingColumn from './TrackingColumn.vue';
import StateColumn from './StateColumn.vue';
const signalValues = ['信号强', '信号一般', '信号弱'];
const batteryLevels = ['100%','90%','80%','70%','60%','50%','40%','30%','20%','10%','0%'];
export default{
    components:{
        TrackingColumn,
        StateColumn
    },
    props:{
        drone_name: ""
    },
    methods:{
        setDroneName(name){
            this.drone_name = name;
        }
    },
    data(){
        return {
            signalValues,
            batteryLevels
        }
    }
}
</script>
<template>
    <div id="data">
        <div id="state-data">
            <StateColumn icon="./src/components/state-card-assets/signal.svg" :values='signalValues' />
            <StateColumn icon="./src/components/state-card-assets/battery.svg" :values='batteryLevels' />
        </div>
        <div id="info-data">
            <div style="grid-column: 1; grid-row: 1;">
                <span style="font-size: 20px; color: #4477CE; font-weight: 600; display: block;">名称</span>
                <span style="font-size: 16px; color: var(--day-night-font-color);">{{ drone_name }}</span>
            </div>
            <div style="grid-column: 2; grid-row: 1;">
                <TrackingColumn icon="./src/components/state-card-assets/speed-meter.svg" title="实时速度" />
            </div>
            <div style="grid-column: 1; grid-row: 2;">
                <TrackingColumn icon="./src/components/state-card-assets/position.svg" title="实时位置" />
            </div>
        </div>
    </div> <!--状态数据--->
</template>
<style scoped>
#data{
    width: 450px;
    height: 250px;
    position: absolute;
    bottom: 40px;
    right: 20px;
    border-radius: 14px;
    display: flex;
    flex-direction: column;
    padding: 10px;
    row-gap: 10px;
    background: linear-gradient(135deg, var(--day-card-color-start), var(--day-card-color-end)),
                linear-gradient(270deg, var(--day-card-color-end) 20%, var(--day-card-color-start) 80%);
    border-radius: 20px;
    border: 1px solid rgba(255,255,255,0.3);
    backdrop-filter: blur(50px);
    -webkit-backdrop-filter: blur(50px);
    box-shadow: 0px 8px 32px 0px rgba(0,0,0,0.37);
}
#state-data{
    display: grid;
    grid-template-columns: repeat(10, 30%);
    grid-template-rows: 100%;
    column-gap: 10px;
    width: 100%;
    height: fit-content;
}
#info-data{
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr 1fr;
    row-gap: 20px;
}
.info-helper-icon{
    width: 32px;
    height: 32px;
    grid-row: 1;
    grid-column: 2;
}
@media (max-width:580px) {
    #data{
        width: 250px;
        height: 140px;
    }
}
</style>