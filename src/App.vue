<script>
import { ref } from 'vue';
export default {
  name: 'App',
  setup() {
    const items = ref([
      {
        id: 0,
        title: 'Num1',
        categoryId: 0,
        time: 0,
        strId: 1,
      },
      {
        id: 1,
        title: 'Num2',
        categoryId: 0, 
        time: 1,
        strId: 1,
      },
      {
        id: 2,
        title: 'Num3',
        categoryId: 1,
        time: 3,
        strId: 1,
      },
    ])

    function mouseDown(e, item) {
     //if (e.screenX/item.time)
     //alert(e.screenX)
    }

    function onDragStart(e, item) {
      e.dataTransfer.dropEffect = 'move'
      e.dataTransfer.effectAllowed = 'move'
      e.dataTransfer.setData('itemId', item.id.toString())
    }

    function onDrop(e, categoryId, timeId, strId) {
      const itemId = parseInt(e.dataTransfer.getData('itemId'))
      items.value = items.value.map(x => {
        if(x.id == itemId)
          {
            x.categoryId = categoryId
            x.time = timeId
            x.strId = strId
          }
        return x
      })
    }

    function getDT (firstTime, secondTime, pr) {
      let x = new Array;
      let w = 0;
      for (let i = firstTime; i<secondTime; i+=(pr*60)){
        let date = new Date();
        date.setTime(i+'810');
        let sos;
        if (date.getHours() < 10){
          sos = "0" + date.getHours();
        }
        else {
          sos = date.getHours();
        }
        if (date.getMinutes() < 10){
          sos += ":0" + date.getMinutes();
        }
        else {
          sos += ":" + date.getMinutes();
        }
        x.push({
          id: w,  
          time: sos,
        });
        w++;
      }
      return(x);
    }

    let times = getDT(1713232800,1713265200, 15);

    let str = ref ([
      {
          id: 0,
          title: null,
          timeNeed: true
        },
        {
          id: 1,
          title: "Val1",
          timeNeed: false
        },
        {
          id: 2,
          title: "Val2",
          timeNeed: false
        },
        {
          id: 3,
          title: "Val3",
          timeNeed: false
        },
        {
          id: 4,
          title: "Adress4",
          timeNeed: false
        },
    ])

    const categories = ref([
      {
      id: 0,
      title: 'Заявки',
      },
      {
        id: 1,
        title: 'Запланировано',
      },
    ])

    return {
      items,
      categories,
      onDragStart,
      onDrop,
      getDT,
      times,
      str,
      mouseDown
    }
  }
}
</script>

<template>
  <div>
    <div>
      <h1>Название</h1>
      <input v-model="name" type="date" id="start" name="trip-start"  />
    </div>
    <div>
      <div v-for="category in categories"
      :key="category.id">
        <h2>{{ category.title }}</h2>
        
          <div v-for="st in str">
            <span v-if="st.timeNeed==false && category.id > 0" id="st">{{ st.title }}</span>
            <div class="timelist"><span v-if="category.id>0 && st.timeNeed==true">Пост</span>
            <div v-if="st.timeNeed==true" v-for="time in times" id="loser"  @drop="onDrop($event, category.id, time.id, st.id)">
              <span v-if="st.timeNeed==true">{{time.time}}</span>
            </div>
            <div v-if="st.timeNeed==false" v-for="time in times" id="loser"  @drop="onDrop($event, category.id, time.id, st.id)"
            class="droppable"
            @dragover.prevent
            @dragenter.prevent> 
              <div v-resizable><span v-for="item in items.filter(x => x.categoryId == category.id && x.time == time.id && x.strId == st.id)"
      @dragstart="onDragStart($event, item)"
      class="draggable"
      draggable="true"
      @mousedown="mouseDown($event, item)">
        <h5>{{ item.title }}</h5>
      </span></div>
        </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>

</style>