<script>
import FullCalendar from '@fullcalendar/vue'
import resourceTimeGridPlugin from '@fullcalendar/resource-timegrid';
import interactionPlugin, { Draggable } from '@fullcalendar/interaction';
export default {
  components: {
    FullCalendar // make the <FullCalendar> tag available
  },
  data: function() {
    return {
      eventList: [
        'Chinese',
        'English',
        'Geography',
        'Physics'
      ],
      calendarOptions: {
        // 插件注册
        plugins: [
          resourceTimeGridPlugin,
          interactionPlugin,
        ],

        // 默认视图设置
        initialView: 'resourceTimeGridDay',
        height: 'auto',
        contentHeight: 610,

        // 头部设置
        // 自定义按钮
        headerToolbar: false,

        // 时间轴设置
        resources: [
          { id: 'a', title: 'Room A' },
          { id: 'b', title: 'Room B'},
          { id: 'c', title: 'Room C' },
          { id: 'd', title: 'Room D' }
        ],
        allDaySlot: false,
        slotMinTime: '08:00:00',
        slotMaxTime: '22:00:00',
        slotLabelFormat: {
          hour: 'numeric',
          minute: '2-digit',
          meridiem: false,
          hour12:false,
        },

        // 拖放设置
        editable: true,
        droppable: true,

        // 事件设置
        eventColor: 'rgb(96, 191, 96)',
        eventTimeFormat: {
          hour: 'numeric',
          minute: '2-digit',
          meridiem: false,
          hour12: false,
        },

        // 放置回调
        drop: this.handleDrop,

        // 事件点击
        eventClick: this.handleEventClick,

      }
      
    }
  },
  methods: {
    initDraggable(){
      document.addEventListener('DOMContentLoaded', function() {

        var containerEl = document.getElementById('external-events');

        // initialize the external events
        // -----------------------------------------------------------------

        new Draggable(containerEl, {
          itemSelector: '.fc-event',
          eventData: function(eventEl) {
            return {
              title: eventEl.innerText,
              duration: '02:00', //事件持续时间
            };
          }
        });
      });
    },
    handleEventClick(clickInfo) {
      if (confirm(`Are you sure you want to delete the event '${clickInfo.event.title}'`)) {
        clickInfo.event.remove()
        this.eventList.push(clickInfo.event.title)
      }
    },
    handleDrop(info){
      info.draggedEl.parentNode.removeChild(info.draggedEl);
    }
  },
  created(){
    this.initDraggable()
  },
}
</script>

<template>
  <div class='demo-app'>
    <div class='demo-app-sidebar'>
      <div class='demo-app-sidebar-section'>
        <h2>Instructions</h2>
        <ul>
          <li>Take elements that live in Draggable Events and make them drag-and-droppable. When they are dropped on a specific date of a calendar, created a new event</li>
          <li>The default event time is 2 hours</li>
          <li>Drag the mouse at the bottom of the event to change the event duration time</li>
          <li>Click an event to delete it and reback Draggable Events</li>
        </ul>
      </div>
      <div id='external-events'>
        <p>
          <strong>Draggable Events</strong>
        </p>
        <div class='fc-event' v-for="(item, index) in eventList" :key="index">
          <div class='fc-event-main'>{{item}}</div>
        </div>
      </div>
    </div>
    <div class='demo-app-main'>
      <div >
        <div class="title">
          <div>
            <span style="font-size: 24px">课程排班</span>
            <span style="color: #409eff">[从待排班列表中拖入]</span>
          </div>
          <div>
            <button style="background-color：#409eff">发送短信</button>
          </div>
        </div>
        <FullCalendar
          class='demo-app-calendar'
          :options='calendarOptions'
        >
        </FullCalendar>
      </div>
    </div>
  </div>
</template>

<style lang='css'>
h2 {
  margin: 0;
  font-size: 16px;
}
ul {
  margin: 0;
  padding: 0 0 0 1.5em;
}
li {
  margin: 1.5em 0;
  padding: 0;
}
b { /* used for event dates/times */
  margin-right: 3px;
}
.demo-app {
  display: flex;
  min-height: 100%;
  font-family: Arial, Helvetica Neue, Helvetica, sans-serif;
  font-size: 14px;
}
.demo-app-sidebar {
  width: 300px;
  line-height: 1.5;
  background: #eaf9ff;
  border-right: 1px solid #d3e2e8;
}
.demo-app-sidebar-section {
  padding: 2em;
}
.demo-app-main {
  flex-grow: 1;
  padding: 3em;
}
.title{
  border: 1px solid rgb(221, 221, 221);
  border-bottom: none;
  max-width: 1100px;
  padding: 10px;
  margin: 0 auto;
  box-sizing: border-box;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.demo-app-calendar{
  border: 1px solid rgb(221, 221, 221);
  padding: 10px;
}

.fc { /* the calendar root */
  max-width: 1100px;
  margin: 0 auto;
}
#external-events {
  margin: 0 30px;
  padding: 0 10px;
  border: 1px solid #ccc;
  background: #eee;
}

#external-events .fc-event {
  cursor: move;
  margin: 3px 0;
  background-color:  rgb(0, 170, 255);
  color: #fff;
  font-size: 12px;
  border-radius: 5px;
}
.fc-license-message{
  display: none;
}
.fc-view-container *, .fc-view-container ::after, .fc-view-container ::before {
    box-sizing:border-box!important;
}
</style>