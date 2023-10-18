<template>
    <div class="window">
      <slot></slot>
    </div>
  </template>
  
  <script lang="ts">
  import { ref, onMounted, computed } from 'vue';
  import interact from 'interactjs';
  
  export default {
    props: {
      row: Number,
      column: Number
    },
    setup(props) {
      // Computed default width and height based on row and column props
      const defaultWidth = computed(() => {
        if (props.column === 1 || props.column === 3) {
          return Math.max(window.innerWidth * 0.25, 100);
        } else {
          return window.innerWidth * 0.5;
        }
      });
  
      const defaultHeight = computed(() => {
        if (props.row === 1) {
          return window.innerHeight * 0.6667;  // As 2nd row can take up to 1/3 of the space.
        } else {
          return Math.max(window.innerHeight * 0.3333, 20);
        }
      });
  
      onMounted(() => {
        interact('.window')
          .resizable({
            edges: { right: true, bottom: true },
            modifiers: [
              interact.modifiers.restrictSize({
                min: { width: 100, height: 20 }
              })
            ],
            inertia: true
          })
          .draggable({
          modifiers: [
            interact.modifiers.snap({
              targets: [
                function (x, y) {
                  const colSnap = window.innerWidth * 0.25;
                  const rowSnap = window.innerHeight * 0.3333;

                  // Snap to the closest column and row
                  const snappedX = Math.round(x / colSnap) * colSnap;
                  const snappedY = Math.round(y / rowSnap) * rowSnap;

                  return {
                    x: snappedX,
                    y: snappedY,
                    range: 40  // Within 40px, snapping will occur
                  };
                }
              ]
            })
          ],
          inertia: true,
          autoScroll: true,
          onmove: dragMoveListener
        })
          .on('resizemove', function (event) {
            const target = event.target;
            let x = (parseFloat(target.getAttribute('data-x')) || 0);
            let y = (parseFloat(target.getAttribute('data-y')) || 0);
  
            target.style.width = event.rect.width + 'px';
            target.style.height = event.rect.height + 'px';
  
            x += event.deltaRect.left;
            y += event.deltaRect.top;
  
            target.style.transform = 'translate(' + x + 'px,' + y + 'px)';
            target.setAttribute('data-x', x);
            target.setAttribute('data-y', y);
          });
      });

      function dragMoveListener(event: any) {
      const target = event.target;
      const x = (parseFloat(target.getAttribute('data-x')) || 0) + event.dx;
      const y = (parseFloat(target.getAttribute('data-y')) || 0) + event.dy;

      target.style.transform = 'translate(' + x + 'px, ' + y + 'px)';
      target.setAttribute('data-x', x);
      target.setAttribute('data-y', y);
    }
  
      return {
        defaultWidth,
        defaultHeight
      };
    }
  };
  </script>
    
  <style scoped>
  .window {
    /* You can add additional styles for the window component here */
    border: 1px solid black;

    position: absolute;
    /* The computed default width and height will be applied here. 
    If you're using the Options API instead of the Composition API,
    you can directly use this.defaultWidth and this.defaultHeight. */
    width: var(--default-width);
    height: var(--default-height);
  }
  </style>
  