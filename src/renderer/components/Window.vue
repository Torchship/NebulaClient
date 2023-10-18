<template>
  <div
    class="window-box"
    ref="windowBox"
  >
    <div class="drag-bar" ref="dragBar"></div>
    <!-- rest of your window content here -->
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import interact from 'interactjs';

export default {
  name: "Window",
  setup() {
    const windowBox = ref(null);
    const dragBar = ref(null);

    let offsetX = 0; // for tracking mouse's initial x-position within drag-bar
    let offsetY = 0; // for tracking mouse's initial y-position within drag-bar

    onMounted(() => {
      // ... Resizing logic remains the same ...

      // For Dragging
      interact(dragBar.value)
        .draggable({
          restrict: {
            restriction: 'parent',
            elementRect: { top: 0, left: 0, bottom: 1, right: 1 }
          },
          snap: {
            targets: [
              interact.createSnapGrid({ x: 40, y: 40 })
            ],
            relativePoints: [ { x: 0, y: 0 } ]
          },
          onstart: event => {
            // Getting the initial position where mouse is clicked inside the drag-bar
            offsetX = event.clientX - event.target.parentElement.getBoundingClientRect().left;
            offsetY = event.clientY - event.target.parentElement.getBoundingClientRect().top;
          },
          onmove: event => {
            const target = windowBox.value;
            target.style.left = (event.clientX - offsetX) + 'px';
            target.style.top = (event.clientY - offsetY) + 'px';
          }
        });
    });

    return {
      windowBox,
      dragBar
    };
  }
}
</script>

<style>
.window-box {
  position: absolute;
  top: 0;
  left: 25vw;
  width: 50vw;
  height: 60vh;
  border: 2px solid #576C6E;
}

.drag-bar {
  width: 100%;
  height: 30px;
  background-color: #576C6E;
  cursor: move;
}
</style>
