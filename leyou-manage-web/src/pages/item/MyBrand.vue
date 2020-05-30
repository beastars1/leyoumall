<template>
    <div>
      <template>
        <div>
          <v-layout class="pa-2 pb-2">
            <v-flex xs2>
              <v-btn round color="primary" dark>新增品牌</v-btn>
            </v-flex>
            <v-spacer/>
            <v-flex xs4>
              <v-text-field label="搜索" hide-details append-icon="search" v-model="search"></v-text-field>
            </v-flex>
          </v-layout>
          <v-data-table
            :headers="headers"
            :items="brands"
            :search="search"
            :pagination.sync="pagination"
            :total-items="totalBrands"
            :loading="loading"
            class="elevation-1"
          >
            <template slot="items" slot-scope="props">
              <td class="text-xs-center">{{props.item.id}}</td>
              <td class="text-xs-center">{{props.item.name}}</td>
              <td class="text-xs-center"><img :src="props.item.image"></td>
              <td class="text-xs-center">{{props.item.letter}}</td>
              <td class="text-xs-center">
                <v-btn color="info" flat icon dark>
                  <v-icon>edit</v-icon>
                </v-btn>
                <v-btn color="error" flat icon dark>
                  <v-icon>delete</v-icon>
                </v-btn>
              </td>

            </template>
          </v-data-table>
        </div>
      </template>
    </div>
</template>

<script>
    export default {
      name: "MyBrand",
      data() {
        return {
          totalBrands: 0,
          brands: [],
          loading: false,
          pagination: {},
          search: "",
          headers: [
            { text: '品牌id', align: 'start', sortable: true, value: 'id' },
            { text: '名称', align: 'start', sortable: false, value: 'name' },
            { text: 'LOGO', align: 'start', sortable: false, value: 'image' },
            { text: '首字母', align: 'start', sortable: true, value: 'letter' },
            { text: '操作', align: 'start', sortable: false }
          ],
        }
      },
      created() {
        this.brands = [
          {
            "id": 2032,
            "name": "OPPO",
            "image": "http://img10.360buyimg.com/popshop/jfs/t2119/133/2264148064/4303/b8ab3755/56b2f385N8e4eb051.jpg",
            "letter": "O"
          },
          {
            "id": 2033,
            "name": "飞利浦（PHILIPS）",
            "image": "http://img12.360buyimg.com/popshop/jfs/t18361/122/1318410299/1870/36fe70c9/5ac43a4dNa44a0ce0.jpg",
            "letter": "F"
          },
          {
            "id": 2034,
            "name": "华为（HUAWEI）",
            "image": "http://img10.360buyimg.com/popshop/jfs/t5662/36/8888655583/7806/1c629c01/598033b4Nd6055897.jpg",
            "letter": "H"
          },
          {
            "id": 2036,
            "name": "酷派（Coolpad）",
            "image": "http://img10.360buyimg.com/popshop/jfs/t2521/347/883897149/3732/91c917ec/5670cf96Ncffa2ae6.jpg",
            "letter": "K"
          },
          {
            "id": 2037,
            "name": "魅族（MEIZU）",
            "image": "http://img13.360buyimg.com/popshop/jfs/t3511/131/31887105/4943/48f83fa9/57fdf4b8N6e95624d.jpg",
            "letter": "M"
          }
        ];

        this.totalBrands = 15;

        // 去后台查询
        this.loadBrands();
      },
      watch: {
        search(){
          this.pagination.page = 1;
        },
        pagination: {
          deep: true,
          handler(){
            this.loadBrands();
          }
        }
      },
      methods: {
        loadBrands(){
          this.loading = true;
          this.$http.get("/item/brand/page", {
            params: {
              key: this.search,  // 搜索栏内容
              desc: this.pagination.descending,  // 是否为降序排列
              page: this.pagination.page,  // 当前页
              rows: this.pagination.rowsPerPage,  // 每页条数
              sortBy: this.pagination.sortBy,  // 排序字段
            }
          }).then(response => {
            this.brands = response.data.items;
            this.totalBrands = response.data.total;
            this.loading = false;
          })
        }
      }
    }
</script>

<style scoped>

</style>
