<template>
    <div class="task-container">
    <div 
        class='task-list-title' 
        :data-name='name'
    >
        {{ title }}
    </div>
    <div class="task-list">
        <ul v-bind:class="test">
          <li 
            v-for="(task, index) in list" 
            v-bind:key='task'
            draggable="true"
            @dragstart="handlerDragstart"
            @dragend="handlerDragend"
            @dragenter="handlerDragenter"
            @dragleave="handlerDragleave"
            @dragover="handlerDragover"
            @drop="handlerDrop"
            :data-title='task.title'
            :data-id='task.id'
            :data-old='name'
          >
            Задание {{index + 1}} {{task.title}}
            <button @click="delTask(index, name)">
              Удалить
            </button>
            <button 
              :data-text='task.title' 
              :data-id='task.id' 
              @click="switchModal('edit', name)"
            >
              Редактировать
            </button>
          </li>
        </ul>  
    </div>
    </div>
</template>


<script>

export default {
    name: 'TaskCol',
    props: {
        name: String,
        title: String,
        changeList: Function,
        list: Object,
        arr: Object,
        delTask: Function,
        switchModal: Function,
    },
    methods: {
        handlerDragstart(){
            const draggable = event.target;
            // Добавляем данные к перетаскиваемому элементу
            draggable.style.opacity = '0.5';
            console.log('handlerDragstart')
        },
        handlerDragend() {
            const draggable = event.target;
            // Добавляем данные к перетаскиваемому элементу
            draggable.style.opacity = '1';
            console.log('draggable', draggable.dataset.id)

            const task = {
                id: draggable.dataset.id,
                title: draggable.dataset.title
            }

            const positionElement = document.elementFromPoint(
                event.x, event.y
            )
            const colName = positionElement.dataset.name;

            this.changeList(
                task, 
                draggable.dataset.old, 
                colName
            )

        },
        handlerDragenter(){
            console.log('handlerDragenter')
        },
        handlerDragleave(){
            console.log('handlerDragleave')
        },
        handlerDragover(){
            console.log('handlerDragover')
        },
        handlerDrop(){
            console.log('handlerDrop')
        }
    }
}

</script>

<style scoped>
 .task-container {
    width: 30%;
 }
 .task-list-title {
    background: white;
    padding: 20px;
 }
 .task-list {
    width: 100%;
    min-height: 400px;
    background: white;
  }
  .task-list ul {
    list-style-type: none;
    padding: 10px 0;
  }
  .task-list li {
    text-align: left;
    background: #f6f6f6;
    padding: 5px 15px;
    margin: 15px 30px;
    cursor: pointer;
  }
  .task-list li:hover{
    outline: 1px solid black;
  }
</style>