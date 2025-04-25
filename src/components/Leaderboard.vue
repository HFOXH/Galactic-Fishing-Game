<script setup>
import Market from '../components/Market.vue';
</script>
<template>
    <div :class="[darkMode ? 'bg-black text-white' : 'bg-white text-black']">
        <div :class="['p-6 max-w-xl mx-auto', darkMode ? 'bg-black text-white' : 'bg-white text-black']">
            <div class="fixed top-4 right-4 flex items-center">
                <label for="darkModeToggle"
                    class="relative inline-block w-12 mr-2 align-middle select-none cursor-pointer">
                    <input type="checkbox" id="darkModeToggle" v-model="darkMode" class="hidden peer" />
                    <span class="toggle-line block w-16 h-8 bg-gray-300 rounded-full peer-checked:bg-gray-800">
                    </span>
                    <span
                        class="toggle-circle absolute top-1 left-1 w-6 h-6 bg-white rounded-full transition-all duration-300 ease-in-out peer-checked:translate-x-8 flex items-center justify-center">
                        <i class="fa-solid fa-sun text-yellow-400"></i>
                    </span>
                </label>
            </div>

            <div class="fixed bottom-4 right-4 flex items-center">
                <div :class="['rounded-xl shadow-lg border-4 border-amber-950 hover:shadow-2xl transition cursor-pointer p-2', darkMode ? 'bg-black' : 'bg-white']"
                    @click="showModal = true">
                    <img src="https://santic.netlify.app/assets/img-challenge/treasure.png"
                        :class="['w-16 h-16', darkMode ? 'bg-black' : 'bg-white']">
                </div>
            </div>

            <h1 class="text-3xl font-bold mb-8 text-center">🏆 Leaderboard</h1>

            <div class="hidden justify-center items-end gap-8 text-center mb-6 md:flex" v-if="topThreeOrdered.length">
                <div class="flex flex-col items-center" v-for="player in topThreeOrdered" :key="player.username">
                    <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ4mxZ8IrY2twrjgUK9YeZw7oejKwjRulzg1w&s"
                        class="w-16 h-16 rounded-full border-4 border-green-400 shadow-lg object-cover" />
                    <div class="text-2xl font-bold">{{ player.username }}</div>
                    <p :class="['text-sm', darkMode ? 'text-purple-300' : 'text-purple-700']"><span
                            class="font-semibold">{{ player.xp }}</span> XP</p>
                    <p class="text-sm text-yellow-600"><span class="font-semibold">{{ player.gold }}</span> 🪙 GOLD</p>

                    <div :class="['text-black font-bold text-3xl rounded-t-xl w-40 flex justify-center items-center', darkMode ? 'bg-yellow-400 border-yellow-700' : 'bg-yellow-200 border-yellow-200',
                        player.rank === 1 ? 'h-40' : player.rank === 2 ? 'h-32' : 'h-24'
                    ]">
                        {{ player.rank }}
                    </div>
                </div>
            </div>

            <div class="space-y-4 block md:hidden">
                <div v-for="(player, index) in topThree" :key="player.username"
                    :class="['flex justify-between items-center rounded-xl shadow-md p-4 px-6 border hover:shadow-lg transition', darkMode ? 'bg-yellow-600 border-yellow-700' : 'bg-yellow-200 border-yellow-200']">
                    <div class="flex items-center gap-4">
                        <div
                            :class="['w-10 h-10 font-bold rounded-full flex items-center justify-center', darkMode ? 'bg-yellow-400 text-yellow-700' : 'bg-yellow-400 text-yellow-700']">
                            {{ player.rank }}
                        </div>
                        <div>
                            <p :class="['font-semibold text-lg', darkMode ? 'text-white' : 'text-gray-800']">{{
                                player.username }}</p>
                            <p :class="['text-sm', darkMode ? 'text-gray-900' : 'text-gray-500']">Level {{ player.level
                            }}
                            </p>
                        </div>
                    </div>
                    <div class="text-right">
                        <p :class="['text-sm', darkMode ? 'text-gray-900' : 'text-gray-600']"><span
                                class="font-semibold">{{
                                    player.xp }}</span> XP</p>
                        <p class="text-sm text-yellow-900"><span class="font-semibold">{{ player.gold }}</span> 🪙 GOLD
                        </p>
                    </div>
                </div>
            </div>
            <br class="block md:hidden"><br class="block md:hidden">

            <div class="space-y-4">
                <div v-for="(player, index) in others" :key="player.username"
                    :class="['flex justify-between items-center rounded-xl shadow-md p-4 px-6 border hover:shadow-lg transition', darkMode ? 'bg-gray-800 border-gray-700' : 'bg-white border-gray-200']">
                    <div class="flex items-center gap-4">
                        <div
                            :class="['w-10 h-10 font-bold rounded-full flex items-center justify-center', darkMode ? 'bg-purple-700 text-purple-100' : 'bg-purple-100 text-purple-700']">
                            {{ player.rank }}
                        </div>
                        <div>
                            <p :class="['font-semibold text-lg', darkMode ? 'text-white' : 'text-gray-800']">{{
                                player.username }}</p>
                            <p :class="['text-sm', darkMode ? 'text-gray-400' : 'text-gray-500']">Level {{ player.level
                            }}
                            </p>
                        </div>
                    </div>
                    <div class="text-right">
                        <p :class="['text-sm', darkMode ? 'text-gray-300' : 'text-gray-600']"><span
                                class="font-semibold">{{
                                    player.xp }}</span> XP</p>
                        <p class="text-sm text-yellow-600"><span class="font-semibold">{{ player.gold }}</span> 🪙 GOLD
                        </p>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <div v-if="showModal" class="fixed inset-0 bg-black bg-opacity-60 flex items-center justify-center z-50">
        <div :class="[
            'p-6 rounded-2xl shadow-xl w-8/12 relative max-h-[75vh] overflow-y-auto',
            darkMode ? 'bg-gray-900' : 'bg-white'
        ]">
            <button @click="showModal = false"
                class="absolute top-2 right-3 text-gray-500 hover:text-red-600 text-xl cursor-pointer">&times;</button>

            <Market :darkMode="darkMode" />

            <div class="mt-6 flex justify-end">
                <button @click="showModal = false"
                    class="bg-yellow-500 hover:bg-yellow-600 text-white font-bold py-2 px-4 rounded-xl cursor-pointer">
                    Close
                </button>
            </div>
        </div>
    </div>


</template>

<script>
export default {
    data() {
        return {
            topThree: [],
            topThreeOrdered: [],
            others: [],
            darkMode: false,
            showModal: false,
        }
    },
    mounted() {
        this.getLeaderBoard();
        this.intervalId = setInterval(() => {
            this.getLeaderBoard();
        }, 4000);
    },
    methods: {
        getLeaderBoard() {
            fetch('https://api-game.bloque.app/game/leaderboard')
                .then(res => res.json())
                .then(data => {
                    const top3 = data.players.slice(0, 3);
                    this.topThree = top3;
                    this.topThreeOrdered = [
                        top3.find(p => p.rank === 2),
                        top3.find(p => p.rank === 1),
                        top3.find(p => p.rank === 3)
                    ].filter(Boolean);
                    this.others = data.players.slice(3);
                })
                .catch(err => {
                    console.error("Error al obtener leaderboard:", err);
                });
        },
        toggleDarkMode() {
            this.darkMode = !this.darkMode;
            if (this.darkMode) {
                document.body.classList.add('dark');
            } else {
                document.body.classList.remove('dark');
            }
        }
    }
}
</script>