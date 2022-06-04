<template>
  <div class="wrapper">
    <the-navbar
      :balance="balance"
      @add-balance="showBalanceDialog"
    ></the-navbar>
    <teleport to="body">
      <my-dialog
        v-if="balanceDialog"
        :closable="true"
        @closeModal="hideBalanceDialog"
      >
        <h1>Пополните баланс на сумму:</h1>
        <my-input
          :icon="false"
          place="Сумма!"
          type="number"
          v-model="addBalance"
        ></my-input>
        <small v-if="balanceErr" style="color: red">{{ balanceErr }}</small>
        <my-button text="Пополнить" @click="balanceAdd"></my-button>
      </my-dialog>
    </teleport>
    <main class="main">
      <div class="container">
        <div class="content">
          <div class="sort">
            <my-sort v-model="sort"></my-sort>
            <my-filter v-model="sortFilter" :options="options"></my-filter>
          </div>
          <my-input
            v-model="search"
            :icon="true"
            place="Введите название книги"
            type="text"
          ></my-input>

          <the-books
            :books="searchFilter"
            :load="isLoad"
            @add-cart="addToCart"
          ></the-books>
        </div>

        <div class="cart">
          <Transition name="fade">
            <div
              class="cart-dialog"
              v-if="cartVisible"
              @click.stop="hideCart"
            ></div>
          </Transition>
          <Transition name="cart">
            <the-cart
              :cartItems="cart"
              @cart-close="hideCart"
              @cart-delete="deleteCartItem"
              @bought="bought"
              v-if="cartVisible"
            ></the-cart>
          </Transition>
          <teleport to="body">
            <my-dialog v-if="cartDialog || notEnough">
              <h1 v-if="notEnough">Недостаточно денег для заказа</h1>
              <h1 v-else>Покупка прошла успешно</h1>
              <my-button text="На главную" @click="dialogHide"></my-button>
            </my-dialog>
          </teleport>
        </div>
      </div>
    </main>
    <my-button
      text="Корзина"
      :cart="true"
      :total="cart"
      @show-cart="showCart"
    ></my-button>
  </div>
</template>

<script>
import TheNavbar from "@/components/TheNavbar";
import TheCart from "@/components/TheCart";
import TheBooks from "@/components/TheBooks";
import MyInput from "@/UI/MyInput";
import MyFilter from "@/UI/MyFilter";
import MySort from "@/UI/MySort";
import MyButton from "@/UI/MyButton";
import MyDialog from "@/UI/MyDialog";

export default {
  name: "App",
  components: {
    TheNavbar,
    MyInput,
    MyFilter,
    MySort,
    TheCart,
    TheBooks,
    MyButton,
    MyDialog,
  },
  data() {
    return {
      search: "",
      sortFilter: null,
      books: [],
      options: [],
      cart: [],
      sort: true,
      cartVisible: false,
      cartDialog: false,
      notEnough: false,
      balance: 10200,
      addBalance: null,
      balanceDialog: false,
      balanceErr: false,
      isLoad: false,
    };
  },
  methods: {
    // получаем категории книг
    async getCat() {
      // метод через xhr тоже работает
      const response = await fetch(
        "http://45.8.249.57/bookstore-api/books/categories"
      );
      const data = await response.json();
      this.options = data;
    },
    // получаем все книги
    async getBooks() {
      this.isLoad = true;

      // метод через xhr тоже работает
      // const requestURL = "http://45.8.249.57/bookstore-api/books";
      //
      // function sendRequest(method, url, body = {}) {
      //   return new Promise((resolve, reject) => {
      //     const xhr = new XMLHttpRequest();
      //
      //     xhr.open(method, url);
      //
      //     xhr.responseType = "json";
      //     xhr.setRequestHeader("Content-Type", "application/json");
      //
      //     xhr.onload = () => {
      //       if (xhr.status >= 400) {
      //         reject(xhr.response);
      //       } else {
      //         resolve(xhr.response);
      //       }
      //     };
      //
      //     xhr.onerror = () => {
      //       reject(xhr.response);
      //     };
      //
      //     xhr.send(JSON.stringify(body));
      //   });
      // }
      // sendRequest("POST", requestURL)
      //   .then((data) => {
      //     this.isLoad = false;
      //     this.books = data;
      //     this.books.forEach((b) => {
      //       b.cartCount = 1;
      //     });
      //   })
      //   .catch((err) => console.log(err));

      const response = await fetch("http://45.8.249.57/bookstore-api/books", {
        headers: {
          "Content-Type": "application/json",
        },
        method: "POST",
        body: JSON.stringify({
          get: {},
        }),
      });
      const data = await response.json();
      this.isLoad = false;
      this.books = data;
      this.books.forEach((b) => {
        b.cartCount = 1;
      });
    },
    // событие добавление книг в корзину
    addToCart(book) {
      const itemInCart = this.cart.find((c) => c.name === book.name);
      if (itemInCart) {
        itemInCart.cartCount++;
        let localCount = JSON.parse(localStorage.getItem("cart"));
        let localItem = localCount.find((i) => i.name === book.name);
        localItem.cartCount += 1;
        localStorage.setItem("cart", JSON.stringify(localCount));
      } else {
        this.cart.push(book);
        this.cartStorage(book);
      }
    },
    // сохранение коризны в localStorage
    cartStorage(item) {
      const items = JSON.parse(localStorage.getItem("cart")) || [];
      items.push(item);
      localStorage.setItem("cart", JSON.stringify(items));
    },
    // показ корзины
    showCart() {
      this.cartVisible = true;
      document.body.classList.add("lock");
    },
    // скрытие корзины
    hideCart() {
      this.cartVisible = false;
      document.body.classList.remove("lock");
    },
    // удаление книги из корзины
    deleteCartItem(i) {
      let localItems = JSON.parse(localStorage.getItem("cart"));
      this.cart.splice(i, 1);
      localItems.splice(i, 1);
      localStorage.setItem("cart", JSON.stringify(localItems));
    },
    // действие при покупке
    bought(sum) {
      let localBalance = JSON.parse(localStorage.getItem("balance"));
      if (this.balance < sum) {
        this.notEnough = true;
        this.cartVisible = false;
        return;
      }
      this.cart = [];
      localStorage.removeItem("cart");
      this.cartVisible = false;
      this.balance = this.balance - sum;
      let lostBalance = localBalance - sum;
      localStorage.setItem("balance", lostBalance);
      this.cartDialog = true;
    },
    // скрыть диалоговое окно
    dialogHide() {
      this.cartDialog = false;
      this.notEnough = false;
      document.body.classList.remove("lock");
    },
    // проверка, число при добавление баланса
    isNum() {
      let validate = true;
      const reg = new RegExp("^[0-9]+$");

      if (this.addBalance === null || !this.addBalance.match(reg)) {
        validate = false;
        this.balanceErr = "Пожалуйста введите цифровые значения";
      } else {
        this.balanceErr = "";
      }

      return validate;
    },
    // добавление баланса
    balanceAdd() {
      if (this.isNum()) {
        let localBalance = JSON.parse(localStorage.getItem("balance"));
        this.balance += +this.addBalance;
        let localNewBalance = localBalance + +this.addBalance;
        localStorage.setItem("balance", localNewBalance);
        this.balanceDialog = false;
        this.addBalance = "";
        document.body.classList.remove("lock");
      }
    },
    //  показ диалогового окна с пополнением баланса
    showBalanceDialog() {
      this.balanceDialog = true;
      document.body.classList.add("lock");
    },
    //  показ диалогового окна с пополнением баланса
    hideBalanceDialog() {
      this.balanceDialog = false;
      document.body.classList.remove("lock");
    },
  },
  computed: {
    // Фильтрация по категориям
    filter() {
      return [...this.books].filter((book) => {
        if (this.sortFilter === null) {
          return book;
        } else {
          return book.categoryId === +this.sortFilter;
        }
      });
    },
    // фильтрация по цене
    sortByPrice() {
      // eslint-disable-next-line vue/no-side-effects-in-computed-properties
      return this.filter.sort((a, b) => {
        if (this.sort === true) {
          return a.price - b.price;
        }
        if (this.sort === false) {
          return b.price - a.price;
        }
      });
    },
    // фильтрация по поиску
    searchFilter() {
      return this.sortByPrice.filter((f) =>
        f.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
  },
  mounted() {
    // очищаем класс у боди
    document.body.classList.remove("lock");
    // вызываем функции получения книг
    this.getBooks();
    this.getCat();
    // сохраняем баланс в localStorage
    if (JSON.parse(localStorage.getItem("balance") === null)) {
      localStorage.setItem("balance", this.balance);
    } else {
      const balance = JSON.parse(localStorage.getItem("balance"));
      this.balance = +balance;
    }

    // получаем товар с корзин из localStorage
    if (JSON.parse(localStorage.getItem("cart") === null)) {
      return;
    } else {
      this.cart = JSON.parse(localStorage.getItem("cart"));
    }
  },
};
</script>

<style lang="scss">
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Inter", sans-serif;
}

body {
  overflow-x: hidden;
  min-height: 100vh;
  width: 100%;
  background-color: rgba(0, 0, 0, 0.01);

  &.lock {
    overflow-y: hidden;
  }
}

#app {
  overflow-x: hidden;
}

.wrapper {
  padding: 20px 0;
  position: relative;
}

.container {
  width: 1280px;
  max-width: 100%;
  margin: 0 auto;
  padding: 0 15px;
}

h1 {
  text-align: center;
}

input,
select {
  border: none;
  outline: none;
  background: transparent;
  font: inherit;
}

.sort {
  display: flex;
  align-items: center;
  gap: 30px;
  margin-bottom: 20px;

  @media screen and (max-width: 472px) {
    flex-direction: column;
    gap: 30px;
  }
}

.btn {
  cursor: pointer;
  border: none;
  font: inherit;
  outline: none;
  background: transparent;
  padding: 0.5em;
  border-radius: 11px;

  transition: all 0.3s linear;

  &:active {
    top: 2px;
  }
}

.cart-dialog {
  position: absolute;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.5);
}

.cart-enter-active,
.cart-leave-active {
  transition: all 0.3s linear;
}

.cart-enter-from,
.cart-leave-to {
  transform: translateX(100%);
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
