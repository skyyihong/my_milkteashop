<template>
  <view class="container">
    <unicloud-db ref="udb" v-slot:default="{data, loading, error, options}" :options="options" :collection="collectionList" field="provider,provider_pay_type,uni_platform,status,type,order_no,out_trade_no,transaction_id,user_id,device_id,client_ip,openid,description,err_msg,total_fee,refund_fee,refund_count,refund_list,provider_appid,appid,user_order_success,custom,original_data,create_date,pay_date,notify_date,cancel_date,stat_data" :where="queryWhere" :getone="true" :manual="true">
      <view v-if="error">{{error.message}}</view>
      <view v-else-if="loading">
        <uni-load-more :contentText="loadMore" status="loading"></uni-load-more>
      </view>
      <view v-else-if="data">
        <view>
          <text>支付供应商</text>
          <text>{{options.provider_valuetotext[data.provider]}}</text>
        </view>
        <view>
          <text>支付方式</text>
          <text>{{data.provider_pay_type}}</text>
        </view>
        <view>
          <text>应用平台</text>
          <text>{{data.uni_platform}}</text>
        </view>
        <view>
          <text>订单状态</text>
          <text>{{options.status_valuetotext[data.status]}}</text>
        </view>
        <view>
          <text>订单类型</text>
          <text>{{data.type}}</text>
        </view>
        <view>
          <text>业务系统订单号</text>
          <text>{{data.order_no}}</text>
        </view>
        <view>
          <text>支付插件订单号</text>
          <text>{{data.out_trade_no}}</text>
        </view>
        <view>
          <text>交易单号</text>
          <text>{{data.transaction_id}}</text>
        </view>
        <view>
          <text>用户ID</text>
          <text>{{data.user_id}}</text>
        </view>
        <view>
          <text>device_id</text>
          <text>{{data.device_id}}</text>
        </view>
        <view>
          <text>客户端IP</text>
          <text>{{data.client_ip}}</text>
        </view>
        <view>
          <text>openid</text>
          <text>{{data.openid}}</text>
        </view>
        <view>
          <text>支付描述</text>
          <text>{{data.description}}</text>
        </view>
        <view>
          <text>支付失败原因</text>
          <text>{{data.err_msg}}</text>
        </view>
        <view>
          <text>订单总金额</text>
          <text>{{data.total_fee}}</text>
        </view>
        <view>
          <text>订单总退款金额</text>
          <text>{{data.refund_fee}}</text>
        </view>
        <view>
          <text>当前退款笔数</text>
          <text>{{data.refund_count}}</text>
        </view>
        <view>
          <text>退款详情</text>
          <text>{{data.refund_list}}</text>
        </view>
        <view>
          <text>开放平台appid</text>
          <text>{{data.provider_appid}}</text>
        </view>
        <view>
          <text>DCloud AppId</text>
          <text>{{data.appid}}</text>
        </view>
        <view>
          <text>回调状态</text>
          <text>{{data.user_order_success == true ? '✅' : '❌'}}</text>
        </view>
        <view>
          <text>自定义数据</text>
          <text>{{data.custom}}</text>
        </view>
        <view>
          <text>异步通知原始数据</text>
          <text>{{data.original_data}}</text>
        </view>
        <view>
          <text>创建时间</text>
          <uni-dateformat :threshold="[0, 0]" :date="data.create_date"></uni-dateformat>
        </view>
        <view>
          <text>支付时间</text>
          <uni-dateformat :threshold="[0, 0]" :date="data.pay_date"></uni-dateformat>
        </view>
        <view>
          <text>异步通知时间</text>
          <uni-dateformat :threshold="[0, 0]" :date="data.notify_date"></uni-dateformat>
        </view>
        <view>
          <text>取消时间</text>
          <uni-dateformat :threshold="[0, 0]" :date="data.cancel_date"></uni-dateformat>
        </view>
        <view>
          <text>uni统计相关数据</text>
          <text>{{data.stat_data}}</text>
        </view>
      </view>
    </unicloud-db>
    <view class="btns">
      <button type="primary" @click="handleUpdate">修改</button>
      <button type="warn" class="btn-delete" @click="handleDelete">删除</button>
    </view>
  </view>
</template>

<script>
  // 由schema2code生成，包含校验规则和enum静态数据
  import { enumConverter } from '../../js_sdk/validator/uni-pay-orders.js'
  const db = uniCloud.database()

  export default {
    data() {
      return {
        queryWhere: '',
        collectionList: "uni-pay-orders",
        loadMore: {
          contentdown: '',
          contentrefresh: '',
          contentnomore: ''
        },
        options: {
          // 将scheme enum 属性静态数据中的value转成text
          ...enumConverter
        }
      }
    },
    onLoad(e) {
      this._id = e.id
    },
    onReady() {
      if (this._id) {
        this.queryWhere = '_id=="' + this._id + '"'
      }
    },
    methods: {
      handleUpdate() {
        // 打开修改页面
        uni.navigateTo({
          url: './edit?id=' + this._id,
          events: {
            // 监听修改页面成功修改数据后, 刷新当前页面数据
            refreshData: () => {
              this.$refs.udb.loadData({
                clear: true
              })
            }
          }
        })
      },
      handleDelete() {
        this.$refs.udb.remove(this._id, {
          success: (res) => {
            // 删除数据成功后跳转到list页面
            uni.navigateTo({
              url: './list'
            })
          }
        })
      }
    }
  }
</script>

<style>
  .container {
    padding: 10px;
  }

  .btns {
    margin-top: 10px;
    /* #ifndef APP-NVUE */
    display: flex;
    /* #endif */
    flex-direction: row;
  }

  .btns button {
    flex: 1;
  }

  .btn-delete {
    margin-left: 10px;
  }
</style>
