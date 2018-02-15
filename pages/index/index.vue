<template>
  <section>
    <i-input @on-keyup.enter='search' v-model='inputContent'>
      <i-button slot="append" icon="ios-search" />
    </i-input>
    <CheckboxGroup>
      <Checkbox v-model='types.vga'>
        <span>VGA</span>
      </Checkbox>
      <Checkbox v-model='types.hdmi'>
        <span>HDMI</span>
      </Checkbox>
      <Checkbox v-model='types.dvi'>
        <span>DVI</span>
      </Checkbox>
    </CheckboxGroup>
    <i-table :columns='parsedColumns' :data='filteredData' />
  </section>
</template>

<script>
  import Name from '@/components/index/name'
  import Review from '@/components/index/review'
  import StatusIndicator from '@/components/index/status-indicator'

  const randomBoolean = () => {
    return Math.random() >= 0.5
  }

  export default {
    asyncData(context) {
      const { vga, dvi, hdmi, keyword } = context.route.query

      return {
        types: {
          hdmi: hdmi || false,
          vga: vga || false,
          dvi: dvi || false
        },
        columns: [{
          title: 'Name',
          key: 'name'
        }, {
          title: 'price',
          key: 'price'
        }, {
          title: 'review',
          key: 'review'
        }, {
          title: 'hdmi',
          key: 'hdmi'
        }, {
          title: 'dvi',
          key: 'dvi'
        }, {
          title: 'vga',
          key: 'vga'
        }],
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
        keyword: keyword || '',
        inputContent: keyword || ''
      }
    },
    computed: {
      parsedColumns() {
        return this.columns.map((column) => {
          if (column.key === 'name') {
            return {
              ...column,
              render(createElement, { row, column, index }) {
                return createElement(
                  Name,
                  { props:{ name: row.name, description: row.description } }
                )
              }
            }
          }

          if (column.key === 'review') {
            return {
              ...column,
              render(createElement, { row, column, index }) {
                return createElement(
                  Review,
                  { props:{ review: row.review } }
                )
              }
            }
          }

          if (column.key === 'vga') {
            return {
              ...column,
              render(createElement, { row, column, index }) {
                return createElement(
                  StatusIndicator,
                  { props:{ status: row.vga } }
                )
              }
            }
          }

          if (column.key === 'hdmi') {
            return {
              ...column,
              render(createElement, { row, column, index }) {
                return createElement(
                  StatusIndicator,
                  { props:{ status: row.hdmi } }
                )
              }
            }
          }

          if (column.key === 'dvi') {
            return {
              ...column,
              render(createElement, { row, column, index }) {
                return createElement(
                  StatusIndicator,
                  { props:{ status: row.dvi } }
                )
              }
            }
          }

          return column
        })
      },
      filteredData() {
        return this.tableData.filter((data) => {
          if ((this.types.vga || this.types.hdmi || this.types.dvi) && ((this.types.vga && !data.vga) || (this.types.hdmi && !data.hdmi) || (this.types.dvi && !data.dvi))) return false

          for (let key in data) {
            const text = data[key].toString().toLowerCase()
            if (text.indexOf(this.keyword.toLowerCase()) > -1) return true
          }

          return false
        })
      }
    },
    methods: {
      search() {
        this.keyword = this.inputContent
      },
      formatter(row, column) {
        return (row.review * 5) / 100;
      },
      redirect() {
        this.$router.push({
          name: 'index',
          query: {
            keyword: this.keyword,
            vga: this.types.vga,
            dvi: this.types.dvi,
            hdmi: this.types.hdmi
          }
        })
      }
    },
    watch: {
      keyword() {
        this.redirect()
      },
      types: {
        handler() {
          this.redirect()
        },
        deep: true
      }
    },
    mounted() {
      const { vga, dvi, hdmi, keyword } = this.$route.query

      this.types.vga = (vga || '').toLowerCase() == 'true'
      this.types.dvi = (dvi || '').toLowerCase() == 'true'
      this.types.hdmi = (hdmi || '').toLowerCase() == 'true'
      this.keyword = keyword || ''
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
</style>
