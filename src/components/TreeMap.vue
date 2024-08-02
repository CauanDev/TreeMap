<template>
  <LoadingCircle v-if="loading"/>
    <div class="flex justify-center items-center h-screen bg-gray-100 flex-col">
      <h1 class="font-extrabold text-xl">Programadores <span class="text-[#164e75]">PHP</span> a cada 10.000 Habitantes</h1>
      <canvas id="myCanvas" width="500" height="500" class="border border-black"></canvas>
    </div>
</template>
  
<script>
  import LoadingCircle from "./LoadingCircle.vue"
  export default {
    name: "TreeMap",
    emits: ['getDetails'],
    components:{LoadingCircle},
    props: {
      array: {
        required: true,
        type: Array
      }
    },
    data() {
      return {
        data: [],
        margin: 3,
        fontSize: 12,
        occupiedPositions: [],
        images: [],
        loading:true
      };
    },
    mounted() {
      this.data = this.array;
      this.data = this.data.sort((a, b) => b.size - a.size);
      this.loading = true
      this.draw();
      this.addMouseEvents();
    },
    methods: {
    draw() {
      const canvas = document.getElementById('myCanvas');
      if (canvas.getContext) {
        const ctx = canvas.getContext('2d');
        const canvasWidth = canvas.width;
        const canvasHeight = canvas.height;

        let x;
        let y;
        let noSpaceAvailable = false; // Flag to check if space was available

        const imagePromises = this.data.map(item => this.loadImage(item.imagem));

        Promise.all(imagePromises).then(images => {
          this.images = images;
          this.data.forEach((item, index) => {
            const size = item.size;
            let positionFound = false;
            y = 0;
            x = 0;
            while (!positionFound) {
              if (this.checkPosition(x, y, size, canvasWidth, canvasHeight)) {
                positionFound = true;

                ctx.drawImage(images[index], x, y, size, size);

                ctx.strokeStyle = 'black';
                ctx.lineWidth = 2;
                ctx.strokeRect(x, y, size, size);

                this.occupiedPositions.push({ x, y, size, item, index });
                return;
                } 
                else 
                {
                x++;
                if (x + size > canvasWidth) {
                  x = 0;
                  y++;
                  if (y + size > canvasHeight) {
                    noSpaceAvailable = true;
                    positionFound = true;
                    return; 
                  }
                }
              }
            }
          });

          if (noSpaceAvailable) {
            alert('Não há espaço suficiente para todas as imagens no canvas.');
            return
          }
        }).catch(error => {
          console.error('Erro ao carregar imagens:', error);
        }).finally(() => {
          this.loading = false;
        });
      }
  },
  loadImage(url) {
    return new Promise((resolve, reject) => {
      const img = new Image();
      img.onload = () => resolve(img);
      img.onerror = () => reject(new Error(`Não foi possível carregar a imagem: ${url}`));
      img.src = url;
    });
  },
  checkPosition(x, y, size, canvasWidth, canvasHeight) {
    if (x + size > canvasWidth || y + size > canvasHeight) {
      return false;
    }

    for (let pos of this.occupiedPositions) {
      if (
        x < pos.x + pos.size &&
        x + size > pos.x &&
        y < pos.y + pos.size &&
        y + size > pos.y
      ) {
        return false;
      }
    }
    return true;
  },
  addMouseEvents() {
    const canvas = document.getElementById('myCanvas');
    const ctx = canvas.getContext('2d');

    canvas.addEventListener('mousemove', (event) => {
      const rect = canvas.getBoundingClientRect();
      const x = event.clientX - rect.left;
      const y = event.clientY - rect.top;

      const hoveredSquare = this.occupiedPositions.find(pos => x >= pos.x && x <= pos.x + pos.size && y >= pos.y && y <= pos.y + pos.size );
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      this.occupiedPositions.forEach((pos, index) => {
        ctx.drawImage(this.images[index], pos.x, pos.y, pos.size, pos.size);
        ctx.strokeStyle = 'black';
        ctx.lineWidth = 2;
        ctx.strokeRect(pos.x, pos.y, pos.size, pos.size);
      });

      if (hoveredSquare) {
        const { x, y, size, item } = hoveredSquare;
        const maxFontSize = size * 1.5;
        const textSize = Math.min(maxFontSize / item.text.length, maxFontSize);

        ctx.font = `${textSize}px Arial`;
        ctx.textAlign = 'center';
        ctx.textBaseline = 'middle';

        const textX = x + size / 2;
        const textY = y + size / 2;

        const textWidth = ctx.measureText(item.text).width;
        const textHeight = textSize;

        const padding = 4;

        ctx.fillStyle = 'rgba(0, 0, 0, 0.5)';
        ctx.fillRect(textX - textWidth / 2 - padding, textY - textHeight - padding, textWidth + padding * 2, textHeight + padding * 2);

        ctx.fillStyle = 'white';
        ctx.fillText(item.text, textX, textY - textHeight / 2);

        const sizeTextWidth = ctx.measureText(item.size).width;
        ctx.fillStyle = 'rgba(0, 0, 0, 0.5)';
        ctx.fillRect(textX - sizeTextWidth / 2 - padding, textY + padding, sizeTextWidth + padding * 2, textHeight + padding * 2);

        ctx.fillStyle = 'white';
        ctx.fillText(item.size, textX, textY + textHeight);
      }
    });

    canvas.addEventListener('click', (event) => {
      const rect = canvas.getBoundingClientRect();
      const x = event.clientX - rect.left;
      const y = event.clientY - rect.top;

      const clickedSquare = this.occupiedPositions.find(pos => x >= pos.x && x <= pos.x + pos.size && y >= pos.y && y <= pos.y + pos.size );
      if (clickedSquare) this.$emit('getDetails', clickedSquare.item);
    });
    }
  }
  };
</script>
  