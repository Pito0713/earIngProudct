<template>
  <div>
    <div class="categoryslider">
      <div id="categoryLIst">
        <div class="laterSlider">
          <a>待會名單</a>
          <br />
          <a v-if="!CartInNothing">還沒有看到喜歡的！</a>
          <div v-for="(Product,index) in cartData" :key="Product[1]+index">
            <a style="font-size:1.2vw;" @click="backData (Product[0])">{{Product[2]}}</a>
          </div>
        </div>

        <div class="cartSlidertitle">
          <a>購物車裡有</a>
        </div>
        <div class="cartSlider">
          <div v-for="(Product,index) in CartBackData" :key="Product[1]+index">
            <a
              style="font-size:1.2vw; text-decoration:underline"
              @click="backData (Product[0])"
            >{{Product[2]}}</a>
            <a>{{Product[4]}}個</a>
            <br />
          </div>
        </div>

        <div class="laterSliderRWB">
          <i class="far fa-heart" @click="DropDownOpen()"></i>
          <a class="laterSliderLenght">{{cartDataLength}}</a>
        </div>
      </div>

      <div class="DropDown" :class="{ DropDownOpen: isOpen }">
        <div class="PopUp">
          <div class="closePopUp" @click="DropDownOpen()">
            <a>X</a>
          </div>
          <div class="laterSeeData">
            <div
              v-for="(Product,index) in cartData"
              :key="Product[1]+index"
              @click="backData (Product[0])"
            >
              <div class="laterSeeDatatext">
                <img :src="Product[7]" />
                <a>{{Product[2]}}</a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="Productslider">
      <div>
        <ul class="categoryLIst">
          <li>
            <a @click="CatchProductItem('All')">熱銷</a>
          </li>
          <li>
            <a @click="filterNew()">新品</a>
          </li>
          <li>
            <a @click="CatchProductItem('earing')">耳環</a>
          </li>
          <li>
            <a @click="CatchProductItem('necklace')">項鍊</a>
          </li>
        </ul>
      </div>
      <div class="filterButton">
        <div style="margin: 1vw 2vw">
          <input class="searcher" style="margin:0vw;" placeholder="想要找..." v-model="searchValue" />
          <button style="margin:0vw;" @click="search ()">
            <a>搜尋</a>
          </button>
        </div>
        <div class="sortButton">
          <button @click="sortPrice()">
            <a>價錢高低</a>
          </button>
          <button @click="sortDate()">
            <a>上架時間</a>
          </button>
        </div>
      </div>
      <div class="Product" id="Product">
        <div v-for="(Product,index) in Products" :key="Product[1]+index" class="ProductItem">
          <div class="ProductItemInfo">
            <div class="ProductImg" @click="backData (Product[0])">
              <img :src="Product[7]" />
            </div>
            <a @click="backData (Product[0])">{{Product[2]}}</a>
            <br />
            <div style="padding-top:1vw; display:flex; justify-content: space-between;">
              <a>NT: {{Product[3]}}</a>
              <a style="float:right; margin:3px 5px;">
                <i
                  class="far fa-heart"
                  :class="{fas : Product[5] }"
                  @click="laterSlider(Product[0])"
                ></i>
              </a>
            </div>
          </div>
          <a style="font-size:0.8rem">上架：{{Product[9]}}</a>
        </div>
      </div>
      <div class="productPage">
        <div class="pagination">
          <div @click="pagination(catchData,currentPage-1)">
            <a>上一頁</a>
          </div>
          <div class="pagination" id="productPage">
            <div
              v-for="pageTotal in pageTotals"
              :key="pageTotal[i]"
              @click="pagination(catchData,pageTotal)"
            >
              <a>{{pageTotal}}</a>
            </div>
          </div>
          <div @click="pagination(catchData,currentPage+1)">
            <a>下一頁</a>
          </div>
        </div>
      </div>
    </div>
    <div class="reload" v-show="reload"> <a>載入中</a></div>
  </div>
</template>

<script>
import $ from "jquery";
const moment = require("moment");
export default {
  data: function() {
    return {
      isOpen: false,
      Products: [],
      pageTotals: [],
      paginationTotal: "",
      currentPage: "",
      datapage: 12,
      catchData: [],
      sortRe: true,
      searchValue: "",
      cartData: [],
      cartDataLength: 0,
      CartInNothing: true,
      timer: "",
      CartBackData: [],
      GetlaterSeeData: [],
      GetlaterSeeDatalenght: 0,
      reload: false,
    };
  },
  watch: {
    cartData: function() {
      let vm = this;
      if (vm.cartData[0] === undefined) {
        vm.CartInNothing = !vm.CartInNothing;
      }
    }
  },
  methods: {
    pagination(ProdcutData, currentPage) {
      if (currentPage > this.paginationTotal) {
        document.getElementById("Product").innerHTML = "";
        currentPage = this.paginationTotal;
      } else {
        document.getElementById("Product").innerHTML = "";
      }
      if (currentPage <= 0) {
        currentPage = 1;
      }
      // 全部資料總數
      // 當前比總頁數大的，當前等於總頁數 //防止剛好進位
      // 最小 （當前)1*(單頁資料)4 - 4(起始頁資料) + 1 起始至少為1
      const minData = currentPage * this.datapage - this.datapage + 1;
      // 最大
      const maxData = currentPage * this.datapage;
      // 當前頁面(1) 比 minData(例如：1) 大且又小於 maxData(例如：4) 就push進去新陣列。
      //    建立新陣列//
      for (this.i = 0; this.i <= ProdcutData.length; this.i++) {
        if (this.i >= minData && this.i <= maxData) {
          this.Products.push(ProdcutData[this.i - 1]);
        }
      }
      if (this.currentPage < this.paginationTotal) {
        if (this.currentPage > 0) {
          document.body.scrollTop = 0;
        }
        // console.log(this.currentPage)
      }
      this.currentPage = currentPage;
    },
    pageSelect() {
      const dataTotal = this.catchData.length;
      // 總共有幾頁//
      document.getElementById("productPage").innerHTML = "";
      this.paginationTotal = Math.ceil(dataTotal / this.datapage);
      for (this.i = 1; this.i <= this.paginationTotal; this.i++) {
        this.pageTotals.push(this.i);
      }
    },
    CatchProductItem(Id) {
      if (Id === "All") {
        this.catchData = this.ProdcutData.filter(
          //    用id 屬性篩選我要的
          function(item) {
            return item[0] !== ""; // 篩掉空值
          }
        );
      } else {
        this.catchData = this.ProdcutData.filter(
          //    用id 屬性篩選我要的
          function(item) {
            return item[1] === Id;
          }
        );
      }
      for (let i = 0; i < this.catchData.length; i++) {
        this.catchData[i][9] = moment(this.catchData[i][9]).format(
          "MM/DD/YYYY"
        );
      } // 轉換日期
      document.getElementById("Product").innerHTML = "";
      this.pagination(this.catchData, 1); // 用抓到的資料帶回
      this.pageSelect();
    },
    sort() {
      this.catchData.sort(function(a, b) {
        //重新排列
        return a[0] - b[0];
      });
    },
    sortPrice() {
      if (this.sortRe === true) {
        this.catchData.sort(function(a, b) {
          //重新排列價錢
          return a[3] - b[3];
        });
        // console.log(this.catchData)
        this.sortRe = !this.sortRe; //用true false來設定 大到小或小到大
      } else {
        this.catchData.sort(function(a, b) {
          return a[3] - b[3];
        });
        this.catchData.reverse();
        // console.log(this.catchData)
        this.sortRe = !this.sortRe; //用true false來設定 大到小或小到大
      }
      // console.log(this.sortRe)
      document.getElementById("Product").innerHTML = ""; //清空
      this.pagination(this.catchData, 1); // 用抓到的資料帶回
      this.pageSelect();
      this.sort();
    },
    sortDate() {
      if (this.sortRe === true) {
        this.catchData.sort(function(a, b) {
          return a[9] < b[9] ? 1 : -1; //用天數比較 小於1天 或 大於1天
        });
        // console.log(this.catchData)
        this.sortRe = !this.sortRe;
      } else {
        this.catchData.sort(function(a, b) {
          return a[9] < b[9] ? 1 : -1;
        });
        this.catchData.reverse();
        // console.log(this.catchData)
        this.sortRe = !this.sortRe;
      }
      // console.log(this.sortRe)
      document.getElementById("Product").innerHTML = "";
      this.pagination(this.catchData, 1); // 用抓到的資料帶回
      this.pageSelect();
      this.sort();
    },
    filterNew() {
      this.catchData = this.ProdcutData;
      for (let i = 0; i < this.catchData.length; i++) {
        var day1 = new Date(this.catchData[i][9].replace(/-/g, "/"));
        var day2 = new Date(); // 當前日期
        var day = day2.getTime() - day1.getTime();
        var time = parseInt(day / (1000 * 60 * 60 * 24));
        this.catchData[i].push(time);
        //console.log(this.catchData)
      }
      this.catchData = this.catchData.filter(function(item) {
        return item[10] < 150; // 篩掉大於值180天
      });
      //console.log(this.catchData)
      document.getElementById("Product").innerHTML = "";
      this.pagination(this.catchData, 1); // 用抓到的資料帶回
      this.pageSelect();
    },
    search() {
      let vm = this;
      vm.catchData = vm.ProdcutData;
      var searched = vm.catchData.map(obj => {
        if (
          Object.keys(obj).some(
            () => obj[2].toString().indexOf(vm.searchValue) !== -1
          )
        ) {
          // 用陣列提供的map函式，遍歷出每筆資料
          // 由於object無map、filter函式，使用Object.keys取得object裡的每個property以遍歷每一object裡的內容。而Object提供some函式，只要其中一筆內容符合則回傳true
          // 接著使用obj[property] 取得該物件屬性內的值。
          return obj;
        }
      });
      var Realsearched = searched.filter(function(obj) {
        return obj !== undefined;
      });

      vm.catchData = Realsearched;
      // console.log(this.catchData)
      document.getElementById("Product").innerHTML = "";
      this.pagination(this.catchData, 1);
      this.pageSelect();
    },
    laterSlider(i) {
      let vm = this;
      if (vm.cartData[0] === undefined) {
        vm.CartInNothing = !vm.CartInNothing;
      }
      vm.ProdcutData[i][5] = !vm.ProdcutData[i][5];
      vm.cartData = vm.ProdcutData.filter(function(item) {
        return item[5] === true; // 篩掉出true
      });
      vm.cartDataLength = vm.cartData.length;
      //console.log(this.cartDataLength);
      var data = [this.ProdcutData[i][5]];
      //console.log(data);
      var parameter = {};
      parameter = {
        url:
          "https://docs.google.com/spreadsheets/d/1nXquMbDuBjMx2Eo7qO1XBKNrJBm8xNGRGexuOFozlts/edit#gid=0",
        name: "工作表1",
        data: data.toString(),
        row: this.ProdcutData[i][0]
      };
      $.get(
        "https://script.google.com/macros/s/AKfycbwzGM7BJ8SnGD626ebzQi3xGdBsJzUlOSdiDIkMmBhplN65FtQ/exec",
        parameter
      );
      this.pageSelect(); // 重新載入
    },
    getlaterSeeData() {
      this.cartData = this.ProdcutData.filter(function(item) {
        return item[5] === true; // 篩掉出true
      });
      this.cartDataLength = this.cartData.length;
    },
    backData(i) {
      let vm = this;
      var data = [
        [
          vm.catchData[i][0],
          vm.catchData[i][1],
          vm.catchData[i][2],
          vm.catchData[i][3],
          vm.catchData[i][4],
          vm.catchData[i][5],
          vm.catchData[i][6],
          vm.catchData[i][7],
          vm.catchData[i][8],
          vm.catchData[i][9]
        ]
      ];
      var parameter = {};
      parameter = {
        url:
          "https://docs.google.com/spreadsheets/d/1LhZ_9DO6JNX2Q7DO_HKRVDoGtyRGEp2ne-m_aIlYQq4/edit#gid=0",
        name: "工作表1",
        data: data.toString(),
        row: vm.catchData[i][0],
        column: vm.catchData[i].length
      };
      $.get(
        "https://script.google.com/macros/s/AKfycbzKEwZkfPc610W7d8w8cktq6OO2R8Tfw6GgmHe1aZVGDbkXlGQ/exec",
        parameter
      );
      this.reload = !this.reload
      this.timer = setTimeout(() => {
        //延遲讓後台更新
        location.href = "https://pito0713.github.io/earingSinglePage/";
      }, 1000);
    },
    CartBackDataFun() {
      this.CartBackData = this.CartBackData.filter(
        //    用id 屬性篩選我要的
        function(item) {
          return item[4] !== ""; // 篩掉數量0的空值
        }
      );
      //console.log(this.CartBackData);
    },
    DropDownOpen() {
      this.isOpen = !this.isOpen;
    }
  },
  mounted() {
    fetch(
      "https://script.google.com/macros/s/AKfycbwzGM7BJ8SnGD626ebzQi3xGdBsJzUlOSdiDIkMmBhplN65FtQ/exec"
    )
      .then(res => {
        return res.json();
      })
      .then(Products => {
        this.ProdcutData = Products;
        this.CatchProductItem("All");
        this.getlaterSeeData();
      });
    fetch(
      "https://script.google.com/macros/s/AKfycbxLQARlHh9k7LbV8-ORSmVjIYAJtgphhKXFS0e6ypXmpAWJX8cV/exec"
    )
      .then(res => {
        return res.json();
      })
      .then(CartBackData => {
        this.CartBackData = CartBackData;
        this.CartBackDataFun();
      });
  }
};
</script>

<style scoped lang="scss">
.categoryslider {
  flex: 20%;
  display: block;
  position: relative;
  z-index: 1;
  height: 100%;
  .laterSlider {
    height: 200px;
    padding-top: 2rem;
    border-top: 1px solid var(--border-color);
    background-color: var(--cart-bg-color);
    overflow: auto;
  }
  .cartSlidertitle {
    margin-top: 2rem;
  }
  .cartSlider {
    width: 100%;
    height: 200px;
    overflow: auto;
  }
}
.categoryLIst {
  display: flex;
  align-items: center;
  ul {
    background-color: var(--product-bg-color);
  }
  li {
    padding: 1vw 2vw;
    a {
      font-size: 1rem;
    }
  }
  li:hover {
    border-bottom: 1px var(--border-color) solid;
  }
}
.reload{
  display: block;
  position: fixed;
  top:50%;
  left: calc(50% - 1.5rem);
  background-color: var(--background-color);
  font-size: 2rem;
  z-index: 3;
}
.Product {
  display: flex;
  position: relative;
  flex-wrap: wrap;
}
.Productslider {
  background-color: var(--product-bg-color);
  flex: 80%;
}
.filterButton {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: var(--background-color);
  a {
    font-size: 1rem;
  }
  button {
    margin: 1vw;
    background-color: #ffffff;
    border: var(--border-color) 1px solid;
    color: var(--plat-color);
  }
  .searcher {
    border: var(--border-color) 1px solid;
    margin: 1vw 2vw;
    font-size: 1rem;
  }
}
.ProductItem {
  display: flex;
  flex-direction: column;
  margin: 5px;
  width: calc(25% - 30px);
  padding: 10px;
  background-color: #ffffff;
  img {
    max-width: 100%;
    max-height: 100%;
  }
}
.ProductItemInfo {
  text-align: start;
  color: var(--product-color);
  overflow: hidden;
  text-overflow: ellipsis;
  a {
    white-space: nowrap;
  }
}
.productPage {
  display: flex;
  justify-content: center;
  .pagination {
    display: flex;
    justify-content: center;
    align-items: center;
    a {
      padding: 2vw;
    }
  }
}
.ProductImg {
  height: 20vw;
  display: flex;
  justify-content: center;
  align-items: center;
}
.laterSliderRWB {
  z-index: 2;
  position: fixed;
  bottom: 16vw;
  right: 2vw;
  width: 8vw;
  height: 8vw;
  border-radius: 9999px;
  font-size: 5vw;
  border: 1px solid var(--border-color);
  background-color: var(--background-color);
  display: none;
  justify-content: center;
  align-items: center;
  .laterSliderLenght {
    color: red;
    position: absolute;
    font-size: 5vw;
    top: -60%;
    right: -10%;
  }
}
.DropDown {
  display: none;
}
.DropDownOpen {
  display: block !important;
}
.PopUp {
  position: fixed;
  display: block;
  z-index: 1;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.5);
}
.closePopUp {
  position: absolute;
  left: 80%;
  top: 11%;
  display: block;
  a {
    font-size: 1.5rem;
    font-weight: 900;
    color: var(--background-color);
  }
}
.laterSeeData {
  margin: 30% 20%;
  width: 60%;
  height: 70%;
  background-color: var(--background-color);
  color: var(--text-color);
  position: absolute;
  overflow: auto;
}
.laterSeeDatatext {
  display: flex;
  align-items: top;
  height: 10%;
  margin: 4vw 4vw;
  padding: 1vw;
  border-bottom: 1px solid var(--primary-color);
}
.laterSeeDatatext a {
  padding: 0 4vw;
  font-size: 0.5rem;
}
.laterSeeDatatext img {
  max-width: 70px;
  max-height: 60px;
}
@media screen and (max-width: 769px) {
  .ProductItem {
    width: calc(33% - 30px);
  }
  .ProductImg {
    height: 18vw;
  }
  .laterSliderRWB {
    display: flex;
  }
  .categoryslider {
    flex: 0%;
  }
  .Productslider {
    flex: 100%;
  }
  .laterSlider {
    display: none;
  }
  .cartSlider {
    display: none;
  }
  .cartSlidertitle {
    display: none;
  }
}
@media screen and (max-width: 425px) {
  .ProductItem {
    width: calc(50% - 30px);
  }
  .filterButton {
    a {
      font-size: 0.8rem;
    }
    button {
      margin: 0.5vw;
    }
    .searcher {
      border: var(--border-color) 1px solid;
      margin: 0.5vw 1vw;
      font-size: 0.9rem;
    }
  }
  .ProductImg {
    height: 25vw;
  }
  .categoryLIst {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
}
@media screen and (max-width: 375px) {
  .ProductItem {
    width: calc(100% - 30px);
  }
  .ProductImg {
    height: auto;
  }
}
</style>