<template>
  <section class="container">
    <input @keyup.enter='search($refs.keywordinput.value)' ref='keywordinput' />
    <div>
      VGI <input type="checkbox" v-model='types.vgi' />
    </div>
    <div>
      HDMI <input type="checkbox" v-model='types.hdmi' />
    </div>
    <div>
      DVI <input type="checkbox" v-model='types.dvi' />
    </div>
    <button @click='search($refs.keywordinput.value)'>Search</button>
    <el-table
      :data="filteredData"
      :default-sort = "{prop: 'date', order: 'descending'}"
      style="width: 100%">
      <el-table-column
        prop="name"
        label="Name">
        <template scope="scope">
          {{ scope.row.name }}
          <h4 style="color: gray;">{{scope.row.description}}</h4>
        </template>
      </el-table-column>
      <el-table-column
        prop="price"
        label="Price">
      </el-table-column>
      <el-table-column
        prop="review"
        label="Review">
        <template scope="scope">
          {{ (scope.row.review * 5) / 100 }}
          <i class="fa fa-star"></i>
          {{ scope.row.review }}
        </template>
      </el-table-column>
      <el-table-column
        prop="hdmi"
        label="HDMI">
        <template scope="scope">
          <span class='circle red' :class='{ red: !scope.row.hdmi, green: scope.row.hdmi }' />
        </template>
      </el-table-column>
      <el-table-column
        prop="vga"
        label="VGA">
        <template scope="scope">
          <span class='circle red' :class='{ red: !scope.row.vga, green: scope.row.vga }' />
        </template>
      </el-table-column>
      <el-table-column
        prop="dvi"
        label="DVI">
        <template scope="scope">
          <span class='circle red' :class='{ red: !scope.row.dvi, green: scope.row.dvi }' />
        </template>
      </el-table-column>
    </el-table>
  </section>
</template>

<script>
  const randomBoolean = () => {
    return Math.random() >= 0.5
  }

  export default {
    data() {
      return {
        types: {
          hdmi: true,
          vgi: true,
          dvi: true
        },
        tableData: [{
          name: 'Belkin aaa',
          description: '4-Port USB 3.1 Hub',
          price: 50,
          review: 89,
          hdmi: randomBoolean(),
          vga: randomBoolean(),
          dvi: randomBoolean()
        }, {
          name: 'Belkin bbb',
          description: '4-Port USB 3.1 Hub',
          price: 50,
          review: 89,
          hdmi: randomBoolean(),
          vga: randomBoolean(),
          dvi: randomBoolean()
        },{
          name: 'Belkin ccc',
          description: '4-Port USB 3.1 Hub',
          price: 50,
          review: 89,
          hdmi: randomBoolean(),
          vga: randomBoolean(),
          dvi: randomBoolean()
        },{
          name: 'Belkin ddd',
          description: '4-Port USB 3.1 Hub',
          price: 50,
          review: 89,
          hdmi: randomBoolean(),
          vga: randomBoolean(),
          dvi: randomBoolean()
        }],
        keyword: ''
      }
    },
    computed: {
      filteredData() {
        return this.tableData.filter((data) => {
          if ((!this.types.vgi || !data.vgi) && (!this.types.hdmi || !data.hdmi) && (!this.types.dvi || !data.dvi)) return false

          for (let key in data) {
            const text = data[key].toString().toLowerCase()
            if (text.indexOf(this.keyword.toLowerCase()) > -1) return true
          }

          return false
        })
      }
    },
    methods: {
      search(keyword) {
        this.keyword = keyword
      },
      formatter(row, column) {
        return (row.review * 5) / 100;
      }
    }
  }
</script>

<style>
.circle {
  display: inline-block;
  border-radius: 50%;
  height: 10px;
  width: 10px;
}

.circle.red {
  background: red;
}

.circle.green {
  background: green;
}

.fa.fa-star {
  color: yellow
}

.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
