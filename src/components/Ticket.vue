<template>
  <div class="ticket">
    <div class="container">
      <div class="row">
        <p class="title">Cari jadwal bioskop sekarang juga di sini <br> untuk hari ini</p>
      </div>
      <div class="row search-form">
        <form class="form-inline" role="form">

          <select class="form-control" v-model="selected">
            <option value="">Pilih Kota</option>
            <option v-for="item in kota" v-bind:value="item.id">{{item.kota}}</option>
          </select>

          <button v-on:click="loadData" type="button" class="btn btn-success"><i class="fa fa-icon fa-search"></i>
            Search
          </button>
        </form>
      </div>
      <div class="row" style="padding-right: 0; padding-left: 0">
            <h3 class="date">{{jadwal.date}}</h3>
      </div>
      <div class="row">
        <div class="masonry">
          <div v-for="item in jadwal.data" class="item">
            <div class="col-xs-12 col-sm-12 col-md-12 col-lg-12">
              <div class="thumbnail">
                <img v-bind:src="item.poster" alt="">
                <div class="caption">
                  <h3>{{item.movie}}</h3>
                  <p><span class="label label-success genre">
                {{item.genre}}
              </span></p>
                  <table class="table table-hover">
                    <thead>
                    <tr>
                      <th>Bioskop</th>
                      <th>Jam</th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for="jadwal in item.jadwal">
                      <td>{{jadwal.bioskop}} <h4><span class="label label-danger">{{jadwal.harga}}</span></h4> </td>
                      <td><span v-for="todo in jadwal.jam" class="jam label-primary">
                  {{todo}}
                </span></td>
                    </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'ticket',
    data () {
      return {
        selected: '',
        kota: [],
        jadwal: {}
      }
    },
    beforeCreate () {
      this.$http.get('http://ibacor.com/api/jadwal-bioskop?k=2d8bfbc2380d5479d97df6e80e9d910a').then(response => {
        console.log(response.data)
        this.kota = response.data.data
      }, response => {
        console.log(response)
      })
    },
    methods: {
      loadData: function () {
        this.$http.get('http://ibacor.com/api/jadwal-bioskop?id=' + this.selected + '&k=2d8bfbc2380d5479d97df6e80e9d910a').then(response => {
          console.log(response.data)
          this.jadwal = response.data
        }, response => {
          console.log(response)
        })
      }
    }
  }
</script>

<style scoped>
  .form-inline .form-control {
    width: 50%;
  }

  .search-form {
    margin-bottom: 10px;
    margin-top: 10px;
  }

  .jam {
    margin-right: 2px;
    margin-bottom: 10px;
    padding: 3px;
    /*border: 1px solid #3498db;*/
    /*background: #3498db;*/
    border-radius: 6px;
    color: white;
  }

  .masonry { /* Masonry container */
    column-count: 3;
    column-gap: 1em;
  }

  .item { /* Masonry bricks or child elements */
    background-color: #eee;
    display: inline-block;
    margin: 0 0 1em;
    width: 100%;
  }

  .title {
    margin-top: 20px;
    font-weight: bold;
    font-size: 24px;
    color: #465665;
  }

  .date{
    margin-left: 15px;
    margin-right: 15px;
    padding-bottom: 10px;
    margin-bottom: 30px;
    border-bottom: 1.5px dashed #bbbbbb;
  }
</style>
