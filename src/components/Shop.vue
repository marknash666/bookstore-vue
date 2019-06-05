<template>
  <div class="shop">
    <ul class="shop_ul">
      <li class="shop_li" v-for="(book,index) in books">
        <img :src="'../static/pics/' + book.title+'.jpg'" alt="good" class="good_img" @mouseover="changeFlagTrue(index)" />
        <div class="good_info">
          <p @click="changeSelectedItem(index)">
            <router-link to="/BookItem" class="title_info">{{book.title}}</router-link>
          </p>
          <p class="good_info_bottom">
            <span class="user_info"><img :src="'../static/images/' + 'shop_jd.png'" alt="shop"/><span class="user_name">{{book.category}}</span></span>
            <span class="price_info"><img :src="'../static/images/price.png'" alt="price"/><span class="price_num">{{book.price}}</span></span>
          </p>
        </div>

        <div class="layout" v-if="book.flag" @mouseout="changeFlagFalse(index,$event)" @click="changeSelectedItem(index)">
          <router-link to="/BookItem" tag="div" class="link">
            <!-- 这里的：class绑定的背景图，对应css在islike.css中 -->
          </router-link>
        </div>
      </li>
    </ul>
  </div>
</template>
<script type="text/javascript">
import {mapGetters} from 'vuex'
import axios from 'axios'
import Qs from 'qs'

	export default {
		data (){
			return {
        books: {
          picture:'../static/images/book_1.jpg',
          title:'并行程序设计导论',
          shopsSrc:'../static/images/shop_jd.png',
          category:'京东自营',
          price:38.70,
          content:'涵盖并行软件和硬件的方方面面，手把手教你如何利用MPI、PThread 和OpenMP开发高效的并行程序',
          author:'陈虎',
          flag: false ,
          publisher:'美国出版社',
          priced:250,
          src: ""
        }
			}
		},
		created (){
			this.$store.dispatch('changeShow','shop')//此步改变导航栏
      var _this =this;
        axios.get("http://localhost:8080/showBooks")
          .then(res => {
            _this.books = res.data.list
            console.log('print result data',res.data.list);
          })
          .catch(function (error) {
            console.log(error);
          })

      console.log('print book');
      console.log(this.books);
		},
		computed:mapGetters({
				goods:'getGoods',
      //books:'getBooks'
			}),
		methods:{
			changeLike(index){
				this.$store.dispatch('changeLike',index)//改变是否喜欢
			},
			changeFlagTrue(index){
				this.$store.dispatch('changeBookFlagTrue',index)//改变是否显示喜欢
			},
			changeFlagFalse(index,e){
				var evt=this.getRelatedTarget(e)
				if(evt.getAttribute('class')){
					if(!(evt.getAttribute('class').indexOf('like')!=-1)){
						this.$store.dispatch('changeBookFlagFalse',index)//改变是否显示喜欢
					}	
				}
			},
			changeSelectedItem(index){
				this.$store.dispatch('changeSelectedBookItem',index)//改变进入商品
			},
			getRelatedTarget: function (event) {
		        if (event.relatedTarget) {
		            return event.relatedTarget;
		        } else if (event.toElement) {
		            return event.toElement;
		        } else if (event.fromElement) {
		            return event.fromElement;
		        } else {
		            return null;
		        }
    		}
		}
	}
</script>
<style scoped>
	.shop{width: 1000px;margin: 0 auto;padding-top: 20px}
	.shop:after{width: 0;height: 0;display: block;clear:both;content: ""}
	.shop_li{float:left;width: 322px;padding: 10px;border:1px solid #d9d9d9;margin-left:10px;margin-bottom: 20px;position: relative;box-sizing: border-box;}
	.good_img{width: 300px;height: 300px;border:none;}
	.title_info{display: inline-block;width: 300px;margin: 4px 0;overflow: hidden;text-overflow: ellipsis;white-space: nowrap;height: 24px;line-height: 24px;font-size:16px; }
	.good_info_bottom{height: 24px;display: block}
	.good_info_bottom:after{width: 0;height: 0;display: block;clear:both;content: ""}
	.price_info,.user_info{height: 24px;display: inline-block}
	.user_info{float:left;}
	.price_info{vertical-align: top;float:right;}
	.user_info img{width: 24px;height: 24px;margin-right:5px ;border: none}
	.price_info img{width: 18px;height: 18px;border: none;}
	.user_name{color:rgb(114, 113, 118);font-size: 12px;vertical-align: top;line-height: 24px;height: 24px;display: inline-block;}
	.price_num{color: #2C2C2C;font-size: 21px;line-height: 24px;font-weight: bold;height: 24px;display: inline-block;margin-left:3px;}
	.layout{z-index:2;width: 300px;height: 300px;opacity: 0.8;background: #111;position: absolute;top:10px;left: 10px;opacity: 0.8;cursor:pointer;}
	.layout p{text-align: right;}
	.layout img{width: 21px;height: 20px;margin-right:5px ;border: none;}
	.layout .like_num{color:#fff;font-size: 18px;vertical-align: top;line-height: 20px;height: 20px;display: inline-block;font-weight: bold}
	 .like{top:115px;left: 50%;margin-left: -35px;position: absolute;width: 70px;height: 90px;    z-index: 9999;opacity: 1;cursor: pointer;}
	 .link{width: 290px;height: 290px;padding: 10px 10px 0 0}
	</style>
