<template>
  <div class="ContactInfo">
    <div class="ContactInfo-userInfo">
      <div class="ContactInfo-userInfo-head">
        <img :src="userInfo.headUrl" alt>
        <p>{{userInfo.userName}}</p>
      </div>
      <ul>
        <li v-if="userInfo.company">
          <div>
            <img width="28" src="../../assets/img/icon_client.png" alt>
          </div>
          <span>{{userInfo.company}}</span>
        </li>
        <li v-if="userInfo.duty">
          <div>
            <img width="28" src="../../assets/img/icon_work.png" alt>
          </div>
          <span>{{userInfo.duty}}</span>
        </li>
        <li v-if="userInfo.email">
          <div>
            <img width="28" src="../../assets/img/email.png" alt>
          </div>
          <span>{{userInfo.email}}</span>
        </li>
        <li v-if="userInfo.address">
          <div>
            <img width="28" src="../../assets/img/location2.png" alt>
          </div>
          <span>{{userInfo.address}}</span>
        </li>
        <li v-if="userInfo.hourRate!='0'">
          <div>
            <img width="28" src="../../assets/img/icon_fate.png" alt>
          </div>
          <span>{{userInfo.hourRate}}元/小时</span>
        </li>
      </ul>
    </div>
    <div class="ContactInfo-calendarinfo">
      <h4>最近日程</h4>
      <img v-show="userCalInfo.length==0" src="../../assets/img/anpai.png" alt>
      <div class="ContactInfo-calendarinfo-list" v-for="(item, index) in userCalInfo" :key="index">
        <p>{{item.beginTime}}</p>
        <span v-for="(childItem, index) in item.calendarlist" :key="index">{{childItem.titleShow}}</span>
      </div>
    </div>
    <button class="delete_btn" @click="isDelete()">删除好友</button>
  </div>
</template>

<script>
import calendarList from "base/CalendarList";
import postHttp from "../../assets/js/postHttp.js";
import { MessageBox } from "mint-ui";

export default {
  components: {
    calendarList
  },
  data() {
    return {
      userId: "",
      userInfo: "",
      userCalInfo: []
    };
  },
  created() {
    this.userInfo = this.$route.query.userinfo;
    this.getFriendCalendar();
    
  },
  methods: {
    async getFriendCalendar() {
      const { data } = await postHttp.post("/Calendar/getFriendCalendar", {
        loginUserId: window.localStorage.getItem("loginUserId"),
        logintoken: window.localStorage.getItem("logintoken"),
        userId: this.userInfo.userId
      });
      if (!data.error) {
        this.userCalInfo = data.data;
      } else {
        alert(data.message);
      }
    },
    isDelete() {
      MessageBox.confirm("确定删除此好友?").then(action => {
        this.deleteFriend();
      });
    },
    async deleteFriend() {
      const { data } = await postHttp.post("/Calendar/getFriendCalendar", {
        loginUserId: window.localStorage.getItem("loginUserId"),
        logintoken: window.localStorage.getItem("logintoken"),
        id: this.userInfo.userId
      });
      if (!data.error) {
        this.userCalInfo = data.data;
      } else {
        alert(data.message);
      }
    },

    
  }
};
</script>

<style lang="less" scoped>
@import "../../assets/css/flex.less";
.ContactInfo {
  width: 100%;
  &-calendarinfo {
    padding-bottom: 40px;
    width: 100%;
    img {
      margin-top: 10px;
      width: 100%;
    }
    h4 {
      margin-top: 10px;
      margin-left: 20px;
    }
    &-list {
      font-size: 14px;
      border-bottom: 1px solid #ededed;
      padding: 10px 0px;
      p {
        font-size: 15px;
        font-weight: bold;
        margin-left: 30px;
      }
      span {
        .f-d-f;
        .f-fd-c;
        margin-top: 7px;
        margin-left: 40px;
        margin-right: 40px;
        padding: 3px 0px;
        border-bottom: 1px #eeddee solid;
      }
    }
  }
  &-userInfo {
    width: 100%;
    &-head {
      margin-top: 70px;
      width: 100%;
      .f-d-f;
      .f-fd-c;
      .f-ai-c;
      img {
        width: 50px;
        height: 50px;
        border-radius: 50%;
      }
      p {
        margin-top: 5px;
      }
    }
    ul {
      width: 100%;
      .f-d-f;
      .f-fd-c;
      li {
        height: 42px;
        .f-d-f;
        .f-ai-c;
        padding: 0 10px;
        border-bottom: 1px solid #e5e5e5;
        span {
          color: #333;
          font-size: 14px;
          margin-left: 10px;
          .f-f-1;
          overflow: hidden;
          text-overflow: ellipsis;
        }
        .bgc {
          color: #2d75ee;
        }
        div {
          width: 28px;
          .f-d-f;
          .f-fd-c;
          .f-ai-c;
        }
      }
    }
  }
}
.delete_btn {
  background: red;
  position: fixed;
  bottom: 0;
  width: 100%;
  height: 35px;
  font-size: 14px;
  color: white;
}
</style>
