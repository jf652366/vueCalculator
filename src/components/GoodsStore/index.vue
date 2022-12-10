<template>
  <template v-if="books.length > 0">
    <table>
      <thead>
        <tr>
          <th>序号</th>
          <th>书籍名称</th>
          <th>出版日期</th>
          <th>价格</th>
          <th>购买数量</th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody>
        <tr
          :class="{ active: activeIndex === index }"
          v-for="(item, index) in books"
          :key="item.id"
          @click="rowClick(index)"
        >
          <td>{{ index + 1 }}</td>
          <td>{{ item.name }}</td>
          <td>{{ item.date }}</td>
          <td>{{ formatPrice(item.price) }}</td>
          <td>
            <button :disabled="item.count <= 1" @click="decrement(item)">
              -
            </button>
            {{ item.count }}
            <button @click="increment(item)">+</button>
          </td>
          <td>
            <button @click="removeItem(index)">移除</button>
          </td>
        </tr>
      </tbody>
    </table>
    <h2>总价：{{ formatPrice(totalPrice) }}</h2>
  </template>

  <template v-else>
    <h1>购物车为空, 请添加喜欢的书籍~</h1>
    <p>商场中有大量的IT类的书籍, 请选择添加学习, 注意保护好自己的头发!</p>
  </template>
</template>


<script>
import { books } from "./mock.js";

export default {
  name: "index",
  data: function () {
    return {
      books,
      activeIndex: 0,
    };
  },
  computed: {
    totalPrice() {
      return this.books.reduce((preValue, item) => {
        return preValue + item.price * item.count;
      }, 0);
    },
  },
  methods: {
    formatPrice(price) {
      return `¥  ${price}`;
    },
    removeItem(index) {
      this.books.splice(index, 1);
    },
    decrement(item) {
      item.count--;
    },
    increment(item) {
      item.count++;
    },
    rowClick(index) {
      this.activeIndex = index;
    },
  },
};
</script>

<style scoped>
table {
  text-align: center;
  border-collapse: collapse;
}

thead {
  background-color: #f5f5f5;
}

th,
td {
  padding: 8px 16px;
  border: 1px solid #aaa;
}
.active {
  background: mediumaquamarine;
}
</style>
