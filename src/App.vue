<template>
  <div class="text-center font-bold mt-10 text-3xl">SPEED TYPING</div>
  <div class="text-center mt-32">
    <div
      class="mb-5 font-semibold text-lg bg-zinc-300 w-32 h-10 rounded-md m-auto p-1 pt-1.5"
    >
      <p class="">
        Time: {{ time_count }}.{{ String(time_count_mc).padStart(2, "0") }}
      </p>
    </div>
    <div
      class="bg-zinc-100 border-2 border-zinc-900 rounded-md inline-block px-3 py-2 pb-2.5 mx-10"
      :class="{
        'text-green-500 font-bold text-2xl duration-500': index == kata.length,
      }"
    >
      <span
        v-for="(kat, index) in kata"
        :id="'kata' + index"
        :class="{
          'text-green-600 font-bold text-xl bg-gray-300/80 bg-center shadow-md mx-1 pl-1 py-1 rounded-md duration-500':
            index_kata == index,
          'font-semibold text-lg duration-500': index_kata != index,
        }"
      >
        {{ kat + " " }}
      </span>
    </div>

    <div class="mt-10">
      <input
        type="text"
        v-model="input_kata"
        v-on:keyup.once="cek_keyboard"
        @keyup.space="keyboard"
        class="p-2 px-3 outline-none bg-zinc-300 drop-shadow-lg focus:drop-shadow-lg focus:bg-zinc-400/50 font-semibold rounded-md"
      />
    </div>
    <div
      :class="{ 'text-red-500 mt-5 font-semibold': input_salah }"
      v-if="input_salah"
    >
      Salah
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      start_time: false,
      time_count: 0,
      time_count_mc: 0,
      intervalId: null,
      interval_mc: null,
      input_salah: false,
      input_kata: "",
      index_kata: 0,
      index: 0,
      kata: [
        "nama",
        "saya",
        "manusia",
        "hello",
        "test",
        "kata",
        "semua",
        "umur",
      ],
    };
  },
  methods: {
    cek_keyboard() {
      this.start_time = true;
      this.intervalId = setInterval(() => {
        this.time_count++;
        this.time_count_mc = 0;
      }, 1000);
      this.interval_mc = setInterval(() => {
        this.time_count_mc++;
        String(this.time_count_mc).padStart(2, "0.");
      });
    },
    keyboard(event) {
      let cek_input = [];
      cek_input.push(this.input_kata.trim());
      // Input Betul
      if (
        cek_input[0] == this.kata[this.index] &&
        cek_input[0] != "" &&
        this.kata[this.index] != undefined
      ) {
        this.index_kata++;
        this.index++;
        this.input_kata = "";
        this.input_salah = false;
        // Input Salah
      } else if (cek_input[0] != this.kata[this.index]) {
        this.input_kata = "";
        if (this.index != this.kata.length) {
          this.input_salah = true;
        }
      }
      if (this.index == this.kata.length && this.start_time == true) {
        console.log(this.index == this.kata.length);
        this.start_time = false;
        clearInterval(this.intervalId);
        clearInterval(this.interval_mc);
      }
    },
  },
  beforeDestroy() {
    clearInterval(this.intervalId);
  },
  computed: {
    randomKata() {
      let addKata = [];
      for (let i = 0; i < this.kata.length; i++) {
        let random = this.kata[Math.floor(Math.random() * this.kata.length)];
        addKata.push(random);
      }
      this.kata = addKata;
      return addKata;
    },
  },
  mounted() {
    this.randomKata;
  },
};
</script>
