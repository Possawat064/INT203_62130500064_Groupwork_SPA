<template>
  <li class="flex flex-row items-center m-2">
    <img
      class="w-15 h-24 rounded-lg"
      :src="require(`../assets/Branch/${DataBranchs.src}`)"
    />
    <span class="text-gray-900 text-opacity-100 font-semibold text-base tracking-widest p-2">{{ DataBranchs.name }}</span>
  </li>
</template>
<script>
export default {
  props: ["DataBranchs"],
};
</script>
