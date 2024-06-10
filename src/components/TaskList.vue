<template>
  <div class='container' 
    :style="{ backgroundImage: `url(${srcList[currentPageBg].src})` }">
  >

    <button @click="toggleMenu">Сменить фон</button>

    <div :class='visibleMenu ? "menu active" : "menu" '>
      <button 
        v-for="(img, index) in srcList" 
        :key="img.id" 
        class='img-button-container'
      >
          <img 
            @click='changeBg' 
            :src='img.src' 
            :data-index="index" 
            class='img-button'
          />
      </button>
    </div>

    <button @click="switchModal('add')">Добавить задание</button>

    <div class="task-list">
        <ul v-bind:class="test">
          <li 
            v-for="(task, index) in list" 
            v-bind:key='task'
            draggable="true"
          >
            Задание {{index + 1}} {{task.title}}
            <button @click="delTask(index)">
              Удалить
            </button>
            <button 
              :data-text='task.title' 
              :data-id='task.id' 
              @click="switchModal('edit', index)"
            >
              Редактировать
            </button>
          </li>
        </ul>  
    </div>
    

    <div class='overlay' v-if="isShowModal">
        <div>
            <form>
              <button class='close' @click="switchModal('close')">
                Закрыть
              </button>
              <div class='form-content'>
                <div>{{ popupp[currunePopupp].title }}:</div>
                <div><input v-model="textNewTask" /></div>
                <button @click="popupp[currunePopupp].click()">
                  {{ popupp[currunePopupp].textInButton }}
                </button>
              </div>
            </form>  
        </div>
    </div>

  </div>
</template>

<script>

export default {
  name: 'task-list',
  data() {
    return {
      currunePopupp: 'add',
      popupp: {
        add: {
          title: 'Добавить задание',
          click: this.addTask,
          textInButton: 'Добавить'
        },
        edit: {
          title: 'Редактировать задание',
          click: this.editTask,
          textInButton: 'Сохранить'
        }
      },
      currentTask: '',
      textNewTask: '',
      isShowModal: false,
      currentPageBg: 0,
      visibleMenu: false,
      srcList: [
        {
          id: 'wqeqwe',
          src: 'https://klevtsovaelena.github.io/wallpaper/img/BlackWhite6.jpg'
        },
        {
          id: 'wqeqqweqwe',
          src: 'https://klevtsovaelena.github.io/wallpaper/img/BlackWhite5.jpg'
        },
        {
          id: 'wqsdfsdfeqwe',
          src: 'https://klevtsovaelena.github.io/wallpaper/img/BlackWhite9.jpg'
        },
        {
          id: 'wqeqsdfsdwe',
          src: 'https://klevtsovaelena.github.io/wallpaper/img/BlackWhite10.jpg'
        }
      ],
      test: 'https://img.freepik.com/free-photo/view-of-3d-adorable-cat-with-fluffy-clouds_23-2151113419.jpg', 
      list: [
        {
          id: '1',
          title: 'Помыть посуду'
        },
        {
          id: '2',
          title: 'Постирать'
        }
      ]
    };
  },
  props: {
    title: String
  },
  methods: {

    toggleMenu() {
        
        if(this.visibleMenu) {
            this.visibleMenu = false
        } else {
            this.visibleMenu = true
        }

    },

    changeBg() {
        const newIndex = event.target.dataset.index;
        // Обновить состояние (поле - currentPageBg)
        this.currentPageBg = newIndex
    },

    addTask() {
        // 1 - остановить перезагрузку страницы
        event.preventDefault();
        // 2 - Получить введенный в инпут текст
        const textInInput = this.textNewTask;
        // 3 - Создадим объект для добавления в массив
        const newObject = {
          title: textInInput
        }
        // 4 - Добавим в массив 
        this.list.push(newObject);
        // 5 - Вызовем функцию для закрытия модального окна
        this.switchModal('close');

    },

    switchModal(type){

      this.currunePopupp = type;
      this.isShowModal = !this.isShowModal;

      switch(type) {
        case 'edit':
          this.textNewTask = event.target.dataset.text
          this.currentTask = event.target.dataset.id
          break;
        case 'add':
          this.textNewTask = ''
          break;
      }

    },

    delTask(index){
        event.preventDefault();
        const newList = this.list.filter((item, indexItem) => {
          return index !== indexItem
        })
        
        // Обновить лист
        this.list = newList
    },
    // ДЗ Редактирование задачи
    editTask(){
      event.preventDefault();
      console.log(this.currentTask)
      console.log(this.textNewTask)
      const newList = this.list.map( (task) => {
        if (task.id === this.currentTask) {
          console.log(task)
          task.title = this.textNewTask
        }
        return task
      })

      this.list = newList
      this.switchModal('close');
    },
  }
}
</script>

<style scoped>
  .task-list {
    width: 30%;
    min-height: 400px;
    background: white;
    padding: 5px;
  }
  .task-list ul {
    list-style-type: none;
    padding: 0;
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
  form .close {
    position: absolute;
    top: 3px;
    right: 3px;
  }
  form{
    position: relative;
    background: white;
    width: 300px;
    height: 300px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .overlay {
    position: fixed;
    top: 0;
    right: 0;
    height: 100vh;
    width: 100%;
    background: rgba(0, 0, 0, 0.6);
    display: flex;
    justify-content: center;
    align-items: center;
  }
  body {
    margin: 0;
  }
  .container {
      position: fixed;
      top: 0;
      right: 0;
      height: 100vh;
      width: 100%;
      background-size: cover;
  }
  .menu {
    position: fixed;
    height: 100vh;
    width: 340px;
    left: -340px;
    transition: 1s;
  }
  .menu.active {
    left: 0;
  }
  .img-button{
    width: 100%;
    height: 100%;
  }
  .img-button-container{
    width: 160px;
    height: 90px;
    padding: 0;
  }
  .img-button img{
    width: 100%;
  }
  .form-content button { 
    margin-top: 10px;
  }
</style>
