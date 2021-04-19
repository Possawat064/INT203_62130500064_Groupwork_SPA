<template>
  <div
    class="flex flex-col justify-center w-3/4 shadow-xl rounded-lg p-2 branch"
  >
    <h1 class="font-semibold text-lg tracking-widest">BRANCHES</h1>
    <p>Find your nearest branches</p>
    <ul>
      <branch-list v-for="i in DataBranchs" :key="i.id" :i="i"></branch-list>
    </ul>
  </div>
</template>

<script>
import BranchList from "../components/BranchList.vue";
export default {
  components: {
    BranchList,
  },
  data() {
    return {
      DataBranchs: [
        {
          name: "CENTRAL@centralwOrld",
          src: "ctw.jpg",
        },
        {
          name: "ICONSIAM",
          src: "icon.jpg",
        },
        {
          name: "CENTRALPLAZA RAMA 2, 1st floor",
          src: "cen2.jpg",
        },
        {
          name: "CENTRALPLAZA WESTGATE , Floor 1",
          src: "cenw.jpg",
        },
        {
          name: "CENTRAL PLAZA BANGNA, 1st Floor",
          src: "ctb.jpg",
        },
      ],
    };
  },
};
</script>
