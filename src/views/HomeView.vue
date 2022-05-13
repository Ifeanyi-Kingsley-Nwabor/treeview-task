<template>
  <div class="treeview">
    <div class="sidebar">
      <ul class="menu-items">
        <li v-for="(item, key, index) in myData" :key="index" class="side-btn">
          <span
            v-if="typeof item === 'object'"
            class="icon"
            @click="Open(item, key)"
            >{{ isExpanded(key) ? "&#9660;" : "&#9658;" }}</span
          >
          <span @click="Open(key)">{{ key }}</span>

          <ul :class="{ hidden: !OpenList.includes(key) }">
            <li
              v-for="(item1, key1, index1) in item"
              :key="index1"
              class="side-btn"
            >
              <span
                v-if="typeof item1 === 'object'"
                class="icon"
                @click="getValue(item1, key, key1)"
                >{{ isExpanded(key1) ? "&#9660;" : "&#9658;" }}</span
              >
              <span @click="getValue(key, key1)">{{ key1 }}</span>
              <ul
                v-if="typeof item1 === 'object'"
                :class="{ hidden: !OpenList.includes(key1) }"
              >
                <li v-for="(item2, key2, index2) in item1" :key="index2">
                  <span class="pointer" @click="getValue1(key, key1, key2)">{{
                    key2
                  }}</span>
                </li>
              </ul>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <div class="content">
      <div class="display-container">
        <label class="label">DE</label>
        <h5 class="display">
          {{ des }}
        </h5>
      </div>
      <div class="display-container">
        <label class="label">EN</label>
        <h5 class="display">
          {{ ens }}
        </h5>
      </div>
    </div>
  </div>
</template>

<script>
import de from "@/locales/en.json";
import en from "@/locales/en.json";

export default {
  name: "HomeView",
  data() {
    return {
      myData: en,
      ens: "",
      des: "",
      OpenList: [],
      OpenNestedList: [],
    };
  },
  methods: {
    isExpanded(key) {
      return this.OpenList.indexOf(key) !== -1;
    },
    Open(key) {
      //check of index already exsists then added it else remove it
      if (this.OpenList.includes(key)) {
        this.OpenList.splice(this.OpenList.indexOf(key), 1);
      } else {
        this.OpenList.push(key);
      }
    },
    getValue(key0, key1) {
      if (typeof en[key0][key1] != "object") {
        this.ens = en[key0][key1];
        this.des = de[key0][key1];
      }
      this.Open(key1);
    },
    getValue1(key0, key1, key2) {
      this.ens = en[key0][key1][key2];
      this.des = de[key0][key1][key2];
    },
  },
  created() {
    console.log(en);
  },
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.hidden {
  display: none;
}
.material-symbols-outlined {
  font-variation-settings: "FILL" 0, "wght" 400, "GRAD" 0, "opsz" 48;
}
.icon {
  margin-right: 5px;
}
ul {
  list-style: none;
  margin-top: 10px;
  width: 200px;
}
li {
  margin: 0 auto;
  padding: 10px;
}
.side-btn {
  border: none;
  cursor: pointer;
  font-size: 16px;
  font-weight: 500;
  color: rgb(16, 13, 13);
  background-color: transparent;
  text-align: start;
  text-transform: uppercase;
}
.sidebar {
  overflow: auto;
  height: 100vh;
  margin-right: 5px;
}
.treeview {
  display: grid;
  grid-template-columns: 1.5fr 4.5fr;
  background-color: rgba(240, 244, 245, 0.5);
}
.content {
  background-color: white;
  border-radius: 10px;
  margin: 6px 6px 6px 0px;
  display: flex;
  align-items: center;
  flex-direction: column;
}

.display-container {
  margin-top: 100px;
  display: flex;
  flex-direction: column;
}

.label {
  font-size: 20px;
  font-weight: 900;
  color: rgba(49, 48, 48, 0.3);
  margin: 10px;
}

.display {
  border: none;
  padding: 20px;
  margin: 30px;
  border-radius: 30px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
}
</style>
