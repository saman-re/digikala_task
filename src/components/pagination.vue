<template>
  <div class="pagination">
    <ul>
      <li class="first numb" :class="{ active: currentPage == 1 }" v-if="!pagesArray.includes(1)" @click="newPage(1)"><span>1</span></li>
      <li class="dots" v-if="!pagesArray.includes(1)"><span>...</span></li>
      <li class="numb" v-for="page in pagesArray" :key="page" :class="{ active: currentPage == page }" @click="newPage(page)">
        <span>{{ page }}</span>
      </li>
      <li class="dots" v-if="!pagesArray.includes(totalPages)">...</li>
      <li
        class="last numb"
        v-if="!pagesArray.includes(totalPages)"
        :class="{ active: currentPage == totalPages }"
        @click="newPage(totalPages)"
      >
        <span>{{ totalPages }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  created() {
    this.newPage(this.currentPage);
  },
  data() {
    return {
      pagesArray: [1],
    };
  },
  methods: {
    // toPersianDigits(item) {
    //   let str = item.toString();
    //   var id = ['۰', '۱', '۲', '۳', '۴', '۵', '۶', '۷', '۸', '۹'];
    //   return str.replace(/[0-9]/g, w => id[+w]);
    // },
    newPage(newPage) {
      this.pagesArray = [];
      this.currentPage=newPage;
      let start = newPage - 2;
      start = start < 1 ? 1 : start;
      for (let i = 0; i < 5 && start+i <= this.totalPages; i++) {
        this.pagesArray.push(start + i);
      }
    },
  },
  computed:{
    totalPages(){
      return this.$store.state.totalPages;
    },
    currentPage:{
      get(){
        return this.$store.getters.getParams.page;
      },
      set(newPage){
      this.$store.commit('changePage',newPage);
      this.$store.dispatch('getProduct');
      }
    }
  }
    // newPage(newPage) {
    //   let lastPage = this.currentPage;
    //   this.currentPage = newPage;
    //   this.$emit('getPage', this.currentPage);
    //   let index = this.pagesArray.indexOf(newPage);
    //   if (index == 0) {
    //     this.pagesArray.pop();
    //     if (newPage > 1) {
    //       this.pagesArray.unshift(newPage - 1);
    //     }
    //   } else if (index == 2 && newPage != this.totalPages) {
    //     this.pagesArray.shift();
    //     if (newPage < this.totalPages) {
    //       this.pagesArray.push(newPage + 1);
    //     }
    //   }
    //   if (newPage == 1) {
    //     this.pagesArray = [];
    //     for (let i = 1; i <= 3 && i <= this.totalPages; i++) {
    //       this.pagesArray.push(i);
    //     }
    //   } else if (newPage == this.totalPages) {
    //     this.pagesArray = [];
    //     for (let i = 0; i < 3 && i < this.totalPages - 1; i++) {
    //       this.pagesArray.unshift(this.totalPages - i);
    //     }
    //   }
    //   if (newPage != lastPage) {
    //     this.$router.push({
    //       query: {
    //         search: this.$route.query.search,
    //         page: newPage,
    //       },
    //     });
      // }
    // },
  // },
};
</script>

<style lang="scss" scoped>
@import '../assets/_variables.scss';
@import url('https://fonts.googleapis.com/css2?family=Lemonada:wght@400;500&display=swap');

.pagination {
  border: $border;
  border-color: $box-border-color;
  ul {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -ms-flex-wrap: wrap;
    flex-wrap: wrap;
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;
    -ms-flex-line-pack: center;
    align-content: center;
    background: #fff;
    padding: 10px;
    margin: 0;
    border-top: $border;
    border-top-color: $box-border-color;
    // border-radius: 8px;
    // -webkit-box-shadow: 0px 10px 15px rgba(0, 0, 0, 0.1);
    // box-shadow: 0px 10px 15px rgba(0, 0, 0, 0.1);

    li.numb {
      list-style: none;
      height: 40px;
      width: max-content;
      padding: 0 12px;
      margin: 0 5px;
      line-height: 40px;
      font-size: 0.986rem;
      border-radius: 8px;
    }

    li {
      color: darken($color: #535353, $amount: 10);
      list-style: none;
      line-height: 45px;
      text-align: center;
      font-family: 'Lemonada', cursive;
      font-size: 10px;
      font-weight: 500;
      cursor: pointer;
      -webkit-user-select: none;
      -moz-user-select: none;
      -ms-user-select: none;
      user-select: none;
      -webkit-transition: all 0.3s ease;
      -o-transition: all 0.3s ease;
      transition: all 0.3s ease;

      &.active,
      &.numb:hover {
        color: #fff;
        background: $secondary;
      }

      &.dots {
        font-size: 22px;
        cursor: default;
      }
    }
  }
}
</style>
