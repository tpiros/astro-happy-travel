<template>
  <form class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4">
    <div class="mb-4">
      <label class="block text-gray-700 text-sm font-bold mb-2" for="to">
        To
      </label>
      <input
        class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
        id="to"
        type="text"
        placeholder="person@example.com"
      />
    </div>
    <div class="mb-6">
      <label class="block text-gray-700 text-sm font-bold mb-2" for="password">
        Text
      </label>
      <textarea
        class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline"
        id="message"
        :value="message"
        rows="5"
        @input="
          debounce(() => {
            message = $event.target.value;
          })
        "
      ></textarea>
    </div>
    <postcard-display :message="message" :image="imageSrc" />
  </form>
</template>
<script>
import { ref } from 'vue';
import PostcardDisplay from './PostcardDisplay.vue';
export default {
  components: { PostcardDisplay },
  name: 'Postcard',
  props: {
    imageSrc: String,
  },
  setup() {
    const message = ref('');
    const image = ref('');
    function debounce() {
      let timeout = null;
      return (cb, delayInMs) => {
        clearTimeout(timeout);
        timeout = setTimeout(() => {
          cb();
        }, delayInMs || 300);
      };
    }

    return {
      debounce: debounce(),
      message,
      image,
    };
  },
};
</script>
