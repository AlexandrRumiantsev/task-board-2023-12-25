<template>
  <div 
    id="draggable" 
    draggable="true"
    @dragstart="handleDragStart"
    @dragend="handleDragEnd"
    @drop="handleDrop"
  >
    Перетаскиваемый элемент
  </div>
</template>

<script>
export default {
  methods: {
    handleDragStart(event) {
      // Получаем элемент, который был перетащен
      const draggable = event.target;
      // Добавляем данные к перетаскиваемому элементу
      draggable.style.opacity = '0.5';
      // Добавляем обработчик события отпускания мыши
      // document.addEventListener('dragend', this.handleDragEnd);
    },
    handleDragEnd() {
      // Удаляем обработчик события отпускания мыши
      // document.removeEventListener('dragend', handleDragEnd);
      // Восстанавливаем стиль перетаскиваемого элемента
      event.target.style.opacity = '1';
    },
    handleDragOver(event) {
      // Предотвращаем стандартное поведение перетаскивания
      event.preventDefault();
    },
    handleDrop(event) {
      // Предотвращаем стандартное поведение перетаскивания
      event.preventDefault();
      // Получаем элемент, на который был брошен перетаскиваемый элемент
      const dropzone = event.target;
      // Удаляем обработчик события отпускания мыши
      // document.removeEventListener('dragend', handleDragEnd);
      // Восстанавливаем стиль перетаскиваемого элемента
      event.target.style.opacity = '1';
      // Перемещаем перетаскиваемый элемент в новую позицию
      event.target.style.transform = `translate(
        ${event.clientX - dropzone.getBoundingClientRect().left}px,
        ${event.clientY - dropzone.getBoundingClientRect().top}px
      )`;
    }
  }
};
</script>

<style>
.dropzone {
  border: 2px dashed #ccc;
  width: 300px;
  height: 200px;
  margin: 20px;
}
</style>