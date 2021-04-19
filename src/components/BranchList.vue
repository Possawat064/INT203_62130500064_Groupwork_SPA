<template>
  <li class="flex flex-row items-center m-2">
    <img
      class="w-18 h-24"
      :src="require(`../assets/Branch/${DataBranchs.src}`)"
    />
    <span class="text-sm p-2">{{ DataBranchs.name }}</span>
  </li>
</template>
<script>
export default {
  props: ["DataBranchs"],
};
</script>
