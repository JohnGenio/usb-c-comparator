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
    <no-ssr>
      <vue-good-table :height='500' :columns='parsedColumns' :rows='filteredData' />
    </no-ssr>
  </section>
</template>

<script>
  import faker from 'faker'
  import Name from '@/components/index/name'
  import Review from '@/components/index/review'
  import StatusIndicator from '@/components/index/status-indicator'

  const randomBoolean = () => {
    return Math.random() >= 0.5
  }

  export default {
    asyncData(context) {
      const { vga, dvi, hdmi, keyword } = context.route.query
      const tableData = []

      for (let i = 0; i < 300; i++) {
        tableData.push({
          name: faker.fake('{{commerce.productName}}'),
          description: '4-Port USB 3.1 Hub',
          price: faker.random.number(),
          review: faker.random.number({min:60, max:100}),
          hdmi: randomBoolean(),
          vga: randomBoolean(),
          dvi: randomBoolean()
        })
      }

      return {
        types: {
          hdmi: hdmi || false,
          vga: vga || false,
          dvi: dvi || false
        },
        columns: [{
          label: 'Name',
          field: 'name',
          sortable: true,
          fixed: 'left'
        }, {
          label: 'price',
          field: 'price',
          type: 'number',
          sortable: true
        }, {
          label: 'review',
          field: 'review',
          type: 'number',
          sortable: true
        }, {
          label: 'hdmi',
          field: 'hdmi',
          type: 'number',
          sortable: true
        }, {
          label: 'dvi',
          field: 'dvi',
          type: 'number',
          sortable: true
        }, {
          label: 'vga',
          field: 'vga',
          type: 'number',
          sortable: true
        }],
        tableData,
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

          if (column.field === 'vga') {
            return {
              ...column,
              render(createElement, { row, column, index }) {
                return createElement(
                  StatusIndicator,
                  { props:{ status: row.vga } }
                )
              },
              sortFn(a) {
                return a ? 1 : -1
              }
            }
          }

          if (column.field === 'hdmi') {
            return {
              ...column,
              render(createElement, { row, column, index }) {
                return createElement(
                  StatusIndicator,
                  { props:{ status: row.hdmi } }
                )
              },
              sortFn(a) {
                return a ? 1 : -1
              }
            }
          }

          if (column.field === 'dvi') {
            return {
              ...column,
              render(createElement, { row, column, index }) {
                return createElement(
                  StatusIndicator,
                  { props:{ status: row.dvi } }
                )
              },
              sortFn(a) {
                return a ? 1 : -1
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
