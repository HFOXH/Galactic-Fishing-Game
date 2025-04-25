<template>
    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4 p-4">
        <div v-for="item in items" :key="item.id"
            class="market-card relative border p-4 rounded-xl shadow-md hover:shadow-xl transition cursor-pointer overflow-hidden"
            :class="darkMode ? 'bg-gray-800 border-amber-700 text-yellow-200' : 'bg-white border-amber-900 text-gray-800'">
            <h3 class="text-lg font-bold mb-2" :class="darkMode ? 'text-yellow-400' : ''">{{ item.name }}</h3>
            <p class="text-sm" :class="darkMode ? 'text-white' : ''">{{ item.description }}</p>
            <p class="mt-2 font-semibold">{{ item.cost.toLocaleString() }} 🪙</p>
        </div>

        <div v-for="n in (9 - items.length)" :key="'empty-' + n" :class="[
            'border border-dashed p-4 rounded-xl opacity-30',
            darkMode ? 'border-amber-600' : 'border-amber-300'
        ]"></div>
    </div>
</template>
<script>
export default {
    props: {
        darkMode: {
            type: Boolean,
            default: false
        }
    },
    data() {
        return {
            items: []
        };
    },
    mounted() {
        this.getMarketItems();
        this.intervalId = setInterval(() => {
            this.getMarketItems();
        }, 4000);
    },
    methods: {
        getMarketItems() {
            fetch('https://api-game.bloque.app/game/market')
                .then(res => res.json())
                .then(data => {
                    this.items = data.items;
                })
                .catch(err => {
                    console.error("Error al obtener leaderboard:", err);
                });
        }
    }
};
</script>
<style>
.market-card {
  position: relative;
  overflow: hidden;
  z-index: 0;
}

.market-card::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  z-index: -1;
  width: 100%;
  height: 100%;
  background: linear-gradient(45deg, yellow, rgb(255, 208, 0), rgb(255, 166, 0), yellow, rgb(255, 208, 0), rgb(255, 166, 0));
  background-size: 800%;
  border-radius: 12px;
  filter: blur(8px);
  opacity: 0;
  transition: opacity 0.3s ease;
}

.market-card:hover::before {
  opacity: 1;
  animation: glowing 20s linear infinite;
}

@keyframes glowing {
  0% {
    background-position: 0 0;
  }
  50% {
    background-position: 400% 0;
  }
  100% {
    background-position: 0 0;
  }
}
</style>

