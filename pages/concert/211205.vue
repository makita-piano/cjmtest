<template>
  <div class="grid-center">
    <div class="col-9_sm-12" style="max-width: 620px;padding: 0 1rem">
      <div>
        <h1>加々見茉耶×木米真理恵デュオリサイタル～二台ピアノの調べ～</h1>
      </div>
      <div class="grid-center col-12">
        <h2 class="col-12">公演日時</h2>
        <div class="col-12">
          <ul class="pre-list">
            <li><p>・2021/12/05(日)</p></li>
            <li><p>・14：00開演(13:30開場)</p></li>
            <li><p>・会場：ヤマハホール(東京都)</p></li>
            <li><p>・［出演］木米真理恵 / 加々見茉耶</p></li>
            <br>
            <li><p>・指定席：5,000円</p></li>
          </ul>
        </div>
        <div class="col-6_sm-12" style="margin-top: 1rem;padding: .5rem" @click="openModal(0)">
          <nuxt-img src="/img/pos1.png" width="768" style="width: 100%;max-width: 768px" />
        </div>
        <div class="col-6_sm-12" style="margin-top: 1rem;padding: .5rem" @click="openModal(1)" >
          <nuxt-img src="/img/pos2.png" width="768" style="width: 100%;max-width: 768px" />
        </div>
        <div v-if="close" class="grid col-12">
          <div class="col-12" style="border: 1px solid #999;padding: 1rem;margin: 1rem .5rem;border-radius: 3px">
            指定席の承りは締切りました。<br>
            当日券もございますので、受付にてお申し付けください。
          </div>
        </div>
        <div v-else class="grid col-12">
          <h2 class="col-12">購入の流れ</h2>
          <div class="col-12">
            <ul class="pre-list">
              <li><p>・座席の選択</p></li>
              <li><p>・個人情報等の記入</p></li>
              <li><p>・決済(銀行振込・PayPay支払)</p></li>
              <li><p>・チケット受け渡し</p></li>
            </ul>
          </div>
          <div class="col-12" style="margin-top: 1rem">
            ※感染症対策のため、黒く塗られた席は選択いただけません。
          </div>
          <div class="col-6_sm-12" @click="openModal(2)" >
            <nuxt-img src="/img/yamaha_1.png" width="768" style="width: 100%" />
          </div>
          <div class="col-6_sm-12" @click="openModal(3)">
            <nuxt-img src="/img/yamaha_2.png" width="768" style="width: 100%"   />
          </div>
          <div class="grid col-12">
            <h2 class="col-12">列選択</h2>
            <div class="col-12">
              希望する席の列記号を選択して下さい。
            </div>
            <div v-for="key in all_seats_keys()" v-on:click="col_click(key)" class="col-3_sm-6" :class="['select-col-' + key, {active: click_col === key}]" :key="key">
              <div style="border: 1px solid #999;padding: 1rem;margin: .5rem">
                {{key}}列({{sold_seats_num()[key]}}/{{all_seats_num()[key]}})
              </div>
            </div>
          </div>
          <div class="grid col-12">
            <h2 class="col-12">席番選択</h2>
            <div v-if="click_col" class='grid col-12'>
              <div v-for="seat in display_seats()" v-on:click="seat_click(seat)" class="col-3_sm-6" :class="['select-seat-' + seat, {active: click_seat === seat}]" :key="seat">
                <div style="border: 1px solid #999;padding: 1rem;margin: .5rem">
                  {{click_col}}列{{seat}}番
                </div>
              </div>
            </div>
            <div v-if="!click_col" class="col-12">
              <span style="color: IndianRed">列を選択してください</span>
            </div>
          </div>
          <div class="grid col-12">
            <h2 class="col-12">氏名(姓)</h2>
            <input class="f-text col-4_sm-8" type="text" v-model="sei" />
          </div>
          <div class="grid col-12">
            <h2 class="col-12">氏名(名)</h2>
            <input class="f-text col-4_sm-8" type="text" v-model="mei">
          </div>
          <div class="grid col-12">
            <h2 class="col-12">座席</h2>
            <p v-if="seat_text">{{seat_text}}</p>
            <p v-else style="color: IndianRed">座席を選択してください</p>
          </div>
          <div class="grid col-12" style="margin-top: 2rem">
            <div v-if="input_check" v-on:click="submit_click()" class="btn" style="margin-bottom: 3rem">
              <p>進む</p>
            </div>
          </div>
        </div>
      </div>
    </div>
    <transition name="modal" appear v-if="modal">
      <div class="modal modal-overlay" @click="closeModal">
        <div class="modal-window" style="max-height: 90vh">
          <div class="modal-content" style="overflow-y: auto"><div style="position: relative;text-align: center"><nuxt-img class="item-img" :src="modal_url" style="max-width: 100%;height: 90vh;" loading="lazy" alt="header-img" /></div></div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    head() {
      return {
        title: "加々見茉耶×木米真理恵デュオリサイタル～二台ピアノの調べ～｜神宮の杜音楽院",
        meta: [
          { hid: 'description', name: 'description', content: '2021/12/05(日)14：00開演 同時期にイモラ国際ピアノアカデミーで学んだ若きピアニスト2人が東京でタッグを組み、挑戦するのは二台ピアノ' },
          { hid: 'og:description', name: 'og:description', content: '2021/12/05(日)14：00開演 同時期にイモラ国際ピアノアカデミーで学んだ若きピアニスト2人が東京でタッグを組み、挑戦するのは二台ピアノ' },
          { hid: 'og:title', property: 'og:title', content: '加々見茉耶×木米真理恵デュオリサイタル～二台ピアノの調べ～｜神宮の杜音楽院' },
          { hid: 'og:image', property: 'og:image', content: 'https://cjmtokyo.com/img/pos1.png' },
        ]
      }
    },
    data () {
      return {
        seats: undefined,
        all_seats: undefined,
        click_col: undefined,
        click_seat: undefined,
        submitted: undefined,
        sei: undefined,
        mei: undefined,
        modal: false,
        item_json: [
          {id: 0, url: '/img/pos1.png'},
          {id: 1, url: '/img/pos2.png'},
          {id: 2, url: '/img/yamaha_1.png'},
          {id: 3, url: '/img/yamaha_2.png'},
        ],
        modal_url: '',
        close: (new Date()).getTime() > (new Date(2021, 11, 5, 0, 0, 0)).getTime()
      }
    },
    created(){
      axios
        .get(process.env.VUE_APP_MICROCMS_URL + "/seats?filters=not_display[equals]false[and]buyer[not_exists]&fields=id,col,row&limit=300",{
          headers: { "X-MICROCMS-API-KEY": process.env.VUE_APP_MICROCMS_KEY},
          data: {}
      })
        .then(v => {
          this.seats = v.data.contents;
      });
      axios
        .get(process.env.VUE_APP_MICROCMS_URL + "/seats?filters=not_display[equals]false&fields=col,row&limit=300",{
          headers: { "X-MICROCMS-API-KEY": process.env.VUE_APP_MICROCMS_KEY},
          data: {}
      })
        .then(v => {
          this.all_seats = v.data.contents;
      });
    },
    methods: {
      all_seats_num: function(){
        var tmp_alpha = {"A": 0,"B": 0,"C": 0,"D": 0,"E": 0,"F": 0,"G": 0,"H": 0,"I": 0,"J": 0,"K": 0,"L": 0,"M": 0,"N": 0,"O": 0,"P": 0,"P": 0,"Q": 0,"R": 0,"S": 0,"T": 0,"U": 0,"V": 0};
        if(this.all_seats != undefined){
            this.all_seats.forEach(element => {
            tmp_alpha[element["col"]] += 1
          });
        }
        for(var key in tmp_alpha){
          if (tmp_alpha[key] == 0){
            delete tmp_alpha[key]
          }
        }
        return tmp_alpha
      },
      all_seats_keys: function(){
        return Object.keys(this.all_seats_num());
      },
      sold_seats_num: function(){
        var tmp_alpha = {"A": 0,"B": 0,"C": 0,"D": 0,"E": 0,"F": 0,"G": 0,"H": 0,"I": 0,"J": 0,"K": 0,"L": 0,"M": 0,"N": 0,"O": 0,"P": 0,"P": 0,"Q": 0,"R": 0,"S": 0,"T": 0,"U": 0,"V": 0};
        if(this.seats != undefined){
            this.seats.forEach(element => {
            tmp_alpha[element["col"]] += 1
          });
        }
        for(var key in tmp_alpha){
          if (tmp_alpha[key] == 0){
            delete tmp_alpha[key]
          }
        }
        return tmp_alpha
      },
      display_seats: function(){
        var result = [];
        if(this.seats != undefined){
          this.seats.forEach(element => {
            if(element["col"] == this.click_col){
              result.push(element["row"]);
            }
          })
        }
        return result
      },
      col_click: function(col){
        this.click_seat = undefined;
        if(this.click_col == col){
          this.click_col = undefined;
        }else{
          this.click_col = col;
        }
      },
      seat_click: function(seat){
        if(this.click_seat == seat){
          this.click_seat = undefined;
        }else{
          this.click_seat = seat;
        }
      },
      submit_click: function(){
        var id = this.seats.find(element => element["col"] == this.click_col && element["row"] == this.click_seat)["id"];
        var params = {
          'blocks': [{
            'type': 'divider'
          }, {
            'type': 'section',
            'text': {
              'type': 'mrkdwn',
              'text': this.click_col + "列" + this.click_seat + "番(" + this.sei + " " + this.mei + ")"
            }
          }]
        }
        axios
          .post(process.env.VUE_APP_SLACK_URL,
                JSON.stringify(params),
                {
                  withCredentials: false,
                  transformRequest: [(data, headers) => {
                    delete headers.common.Authorization
                    delete headers.post['Content-Type']
                    return data
                  }]
                }
          )
          .then()
          .catch(err => {
            console.log(err);
            console.log(err.response);
            console.log(err.message);
          })
        axios
          .patch(process.env.VUE_APP_MICROCMS_URL + "/seats/" + id,
                 {buyer: this.sei + " " + this.mei},
                 {
                   headers: { "X-MICROCMS-API-KEY": process.env.VUE_APP_MICROCMS_WRITE_KEY,
                              "Content-Type": 'application/json'
                            },
                  }
          )
          .then(v => {
            window.location.href = 'https://docs.google.com/forms/d/e/1FAIpQLSeteipSnUbMXzrlQ4czCodns1VKxW4Q6kprznWSpWMjRHgXcg/viewform';
          }).catch(err => {
          });
      },
      openModal(number){
        this.modal_url = this.item_json[number]["url"]
        this.modal = true
      },
      closeModal() {
        this.modal = false
      }
    },
    computed: {
      seat_text: function(){
        if (this.click_col && this.click_seat){
          return this.click_col + '列' +  this.click_seat + '番';
        }
      },
      input_check: function(){
        return this.click_col && this.click_seat && this.sei && this.mei
      }
    }
  }
</script>

<style scoped>
.active > div{
  background: #B18E44;
  color: white;
}
.f-text {
  width: 100%;
  padding: 10px 15px;
  font-size: 16px;
  border-radius: 3px;
  border: 2px solid #ddd;
  box-sizing: border-box;
}
.btn {
  background: MediumSlateBlue;
  border: 1px solid transparent;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  width: 300px;
  height: 60px;
  color: #333;
  font-size: 18px;
  font-weight: 700;
  text-decoration: none;
  transition: 0.3s;
}

.modal-overlay {
  align-items: center;
  background: rgba(0, 0, 0, .5);
  display: flex;
  height: 100%;
  justify-content: center;
  left: 0;
  padding: 5vh 0;
  position: fixed;
  top: 0;
  width: 100%;
  z-index: 30;
}
.modal-window {
  background: #FFF;
  border-radius: 1px;
  overflow: hidden;
  width: 100%;
  margin: auto;
}
.modal-header {
  background: #CEE8E4;
  padding: 10px;
  text-align: left;
}
.modal-enter-active , .modal-leave-active {
  transition: opacity .4s;
}
.modal-enter-active .modal-window {
  transition: opacity .4s, transform .4s;
}
.modal-leave-active .modal-window {
  transition: opacity .4s, transform .4s;
}
.modal-leave-active {
  transition: opacity .6s ease .4s;
}
.modal-enter , .modal-leave-to {
  opacity: 0;
}
.modal-enter .modal-window {
  opacity: 0;
  transform: translateY(-20px);
}
.modal-leave-to .modal-window {
  opacity: 0;
  transform: translateY(-20px);
}

.modal-content p {
  padding-bottom: 1rem;
}

@media (min-width: 769px) {
  .modal-window {
    max-width: 768px;
    width: 100%;
  }
  .modal-overlay {
    padding: 0;
  }
}
</style>