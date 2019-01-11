<template>
    <el-container>
      <el-header :class="$style['el-header']">
          基于EOS区块HASH的抽奖程序
      </el-header>
      <hr/>
      <el-main>
          <el-input-number v-model="num1" :disabled="processing" controls-position="right" size="large" placeholder="抽奖人数" :min="1" :max="10" label="描述文字"></el-input-number>
          <el-button @click="lucky"  :loading="processing">抽奖</el-button>
        <el-table v-if="tableData.length >= 1" :data="tableData">
          <el-table-column :class-name="$style.highlight" :label-class-name="$style.init" prop="luckyNumber" label="中奖号码(十进制列%总人数+1)">
          </el-table-column>
          <el-table-column prop="blockNum" label="区块高度" >
          </el-table-column>
          <el-table-column prop="blockHash" label="区块Hash">
          </el-table-column>
          <el-table-column prop="lastFive" label="Hash后五位">
          </el-table-column>
          <el-table-column prop="lastFive10" label="十进制(Hash后五位/16)">
          </el-table-column>
        </el-table>
      </el-main>
    </el-container>
</template>

<script>
import Eos from "eosjs";
import BN from "bn.js"

const network = {
  blockchain: "eos",
  host: "nodes.get-scatter.com",
  port: 443,
  protocol: "https",
  chainId: "aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e906"
};

// Put eosClient in data will case a weird problem in scatter-desktop.
let eosClient = null;

const requiredFields = { accounts: [network] };

export default {
  name: "lucky",
  data() {
    return {
      readOnlyEos: null,
      num1: 1,
      tableData: [],
      luckyNumber: [],
      timer: null,
      processing: false
    };
  },
  created() {
    let chainId = network.chainId;
    let httpEndpoint =
      network.protocol + "://" + network.host + ":" + network.port;
    this.readOnlyEos = Eos({
      chainId,
      httpEndpoint
    });
    let that = this;
    this.timer = setInterval(that.getinfo, 2000);
  },
  methods: {
    lucky() {
      this.tableData = [];
      this.luckyNumber = [];
      this.processing = true;
    },
    async getinfo() {
      console.log(this.processing);
      if(this.processing) {
        let info = await this.readOnlyEos.getInfo({});
        let luckyItem = {};

        luckyItem["blockNum"] = info.head_block_num;
        luckyItem["blockHash"] = info.head_block_id;
        luckyItem["lastFive"] = info.head_block_id.substring(info.head_block_id.length-5);
        let num16 = new BN(luckyItem["lastFive"], 16);
        let num10 = new BN(num16.toString(10), 10);
        let div16 = num10.div(new BN(16, 10));
        luckyItem["lastFive10"] = div16.toString(10);
        let mod = div16.mod(new BN(window.total, 10));
        let r = mod.add(new BN(1, 10));
        luckyItem["luckyNumber"] = r.toString(10);

        if(!this.luckyNumber.includes(luckyItem['luckyNumber'])){
          this.luckyNumber.push(luckyItem['luckyNumber']);
          this.tableData.push(luckyItem);
        }
        if(parseInt(this.num1) == this.luckyNumber.length) {
          this.processing = false;
        }
      }
      
    },
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style  module>
  button {
    padding: 6px 12px;
  }

  hr {
    width: 100%;
    border: none;
    border-top: 1px solid #f1f1f1;
    padding: 0;
    margin: 0;
  }
  .el-header {
/*    background-color: #B3C0D1;*/
    color: #333;
    line-height: 60px;
    font-size: 20px;
  }
  .highlight {
    font-size: 20px;
    font-weight: 700;
    color: red;
  }
  .init {
    font-size: inherit;
    font-weight: inherit;
    color: inherit;
  }
</style>
