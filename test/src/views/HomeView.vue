<script setup lang="ts">
import { reactive, watch } from 'vue';
import axios from 'axios';
const data= reactive({
  sx:{//面板加点相性等级门派
    dj:142,
    xx:1,
    ll:909,//灵力
    ll2:284,//力量
    mj:481,
    tz:303,
    jxxz:49,
    mxxz:41,
    sxxz:10,
    hxxz:55,
    txxz:9,
  },
  ls:{//洛书
    ws:1330,
    fs:735,
    sd:180,
    fy:2280,
  },
  fl:{//附灵
    ws:495,
    fs:270,
    sd:90,
    fy:990,
    sdbsb:4,
  },
  wh:{//武魂
    ws:1174,
    fs:636,
    sd:184,
    fy:2358,
  },
  hq:{//魂器
    sd:502,
    ws:1245,
    fs:0,
    fy:0,
    qx:0,
    fl:0
  },
  ly:{
    ws:415,
    fs:223,
    qx:1240,
    fy:824,
  },
  ss:{
    sh1:364,
    sh2:364,
  },
  gm:[
    {
      gmop:1,
      value:142
    },
    {
      gmop:1,
      value:0
    },    
    {
      gmop:1,
      value:0
    },
    {
      gmop:1,
      value:0
    },
  ],
  tj:{
    ws:640,
    fs:360,
    qx:280,
  },
  fb:{
    fbxx:1,
    fsStatus:false//角色是否飞升
  },
  zq:{//坐骑属性
    ws:0,
    fs:0,
    fy:0,
  },
  zbsx:{//装备属性
    wqsh:13818,
    wqfs:5504,
    tgfy:2557,
    tgqx:2653,
    tgfl:1398,
    yffy:3341,
    yfqx:4781,
    yffl:2447,
    xzfy:2758,
    xzsd:546
  },
  atz:{//套装暗属性
    tzxx:1,
    tt:{//土套
      ws1:0,
      ws2:0,
      ws3:0,
      ws4:0,
    },
    jt:{
      fs1:604,
      fs2:748,
      fs3:673,
      fs4:524,
    },
    mt:{
      qx1:0,
      qx2:0,
      qx3:0,
      qx4:0,
    },
    st:{
      fy1:0,
      fy2:0,
      fy3:0,
      fy4:0,
    },
    ht:{
      sd1:0,
      sd2:0,
      sd3:0,
      sd4:0,
    }
  }
})
const jsjg = reactive({
  data:{
    options:[
      {
        value: 1,
          label: '金',
          },
          {
            value: 2,
            label: '木',
          },
          {
            value: 3,
            label: '水',
          },
          {
            value: 4,
            label: '火',
          },
          {
            value: 5,
            label: '土',
          },
    ],
    optionsgm:[
      {
        value: 1,
          label: '伤害',
          },
          {
            value: 2,
            label: '速度',
          },
          {
            value: 3,
            label: '防御',
          },
          {
            value: 4,
            label: '气血',
          },
          {
            value: 5,
            label: '法力',
          },
    ]
  },
  jssx:{//计算结果
    zdqxz:105,
    zdflz:84,
    ws:45,
    fs:45,
    sd:50,
    fy:25,
  },
  jssxApi:{//计算结果
    zdqxz:105,
    zdflz:84,
    ws:45,
    fs:45,
    sd:50,
    fy:25,
  },
})
const jsjgFun = ()=>{
      jsjg.jssx.zdqxz = Number(jsjg.jssxApi.zdqxz) + Number(data.hq.qx)+ Number(data.zbsx.tgqx)+ Number(data.zbsx.yfqx)+ Number(data.ly.qx) + Number(data.tj.qx); 
      jsjg.jssx.zdflz = Number(jsjg.jssxApi.zdflz) + Number(data.hq.fl)+ Number(data.zbsx.tgfl)+ Number(data.zbsx.yffl);
      jsjg.jssx.ws = Number(jsjg.jssxApi.ws) + Number(data.ls.ws) + Number(data.fl.ws)+ Number(data.wh.ws)+ Number(data.hq.ws)+ Number(data.zq.ws) + Number(data.zbsx.wqsh) + Number(data.ly.ws) + Number(data.tj.ws) + Number(data.ss.sh1)  + Number(data.ss.sh2);
      jsjg.jssx.fs = Number(jsjg.jssxApi.fs) + Number(data.ls.fs) + Number(data.fl.fs)+ Number(data.wh.fs)+ Number(data.hq.fs) + Number(data.zq.fs) + Number(data.zbsx.wqsh)- Number(data.zbsx.wqfs*0.25) + Number(data.ly.fs)+ Number(data.tj.fs)+ Number(data.ss.sh1)  + Number(data.ss.sh2);
      jsjg.jssx.sd = Number(jsjg.jssxApi.sd) + Number(data.ls.sd) + Number(data.fl.sd)+ Number(data.wh.sd)+ Number(data.hq.sd)+ Number(data.zbsx.xzsd)
      jsjg.jssx.fy = Number(jsjg.jssxApi.fy) + Number(data.ls.fy) + Number(data.fl.fy)+ Number(data.wh.fy)+ Number(data.hq.fy)+ Number(data.zq.fy) + Number(data.zbsx.tgfy) + Number(data.zbsx.yffy) + Number(data.zbsx.xzfy) + Number(data.ly.fy);
      for(let i=0;i<data.gm.length;i++){
        if(data.gm[i].gmop == 1){
          jsjg.jssx.ws = Number(jsjg.jssx.ws) + Number(data.gm[i].value)
          jsjg.jssx.fs = Number(jsjg.jssx.fs) + Number(data.gm[i].value)
        } else if(data.gm[i].gmop == 2){
          jsjg.jssx.sd = Number(jsjg.jssx.sd) + Number(data.gm[i].value)
        } else if(data.gm[i].gmop == 3){
          jsjg.jssx.fy = Number(jsjg.jssx.fy) + Number(data.gm[i].value)
        } else if(data.gm[i].gmop == 4){
          jsjg.jssx.zdqxz = Number(jsjg.jssx.zdqxz) + Number(data.gm[i].value)
        } else if(data.gm[i].gmop == 5){
          jsjg.jssx.zdflz = Number(jsjg.jssx.zdflz) + Number(data.gm[i].value)
        }   
      }
      console.log(jsjg.jssx.fs,'jsjg.jssx.fs')
              // 套装
      switch (data.atz.tzxx) {
        case 1:
          jsjg.jssx.fs = Number(jsjg.jssx.fs) + Number(data.atz.jt.fs1)  + Number(data.atz.jt.fs2) + Number(data.atz.jt.fs3) + Number(data.atz.jt.fs4) 
          console.log(jsjg.jssx.fs,'jsjg.jssx.fs')
          break;
          case 2:
          jsjg.jssx.zdqxz = Number(jsjg.jssx.zdqxz) + Number(data.atz.mt.qx1)  + Number(data.atz.mt.qx2) + Number(data.atz.mt.qx3) + Number(data.atz.mt.qx4)  
          break;
          case 3://水套
          jsjg.jssx.fy = Number(jsjg.jssx.fy) + Number(data.atz.st.fy1)  + Number(data.atz.st.fy2) + Number(data.atz.st.fy3) + Number(data.atz.st.fy4)  
          break;
          case 4://火套
          jsjg.jssx.sd = Number(jsjg.jssx.sd) + Number(data.atz.ht.sd1)  + Number(data.atz.ht.sd2) + Number(data.atz.ht.sd3) + Number(data.atz.ht.sd4)  
          break;
          case 5:
          jsjg.jssx.ws = Number(jsjg.jssx.ws) + Number(data.atz.tt.ws1)  + Number(data.atz.tt.ws2) + Number(data.atz.tt.ws3) + Number(data.atz.tt.ws4)  
          break;
        default:
          break;
      }
        switch (data.fb.fbxx) {
        case 1:
          // jsjg.jssx.fs = (jsjg.jssx.fs) * (100+ 3)/100 
          break;
          case 2:
          jsjg.jssx.zdqxz = (jsjg.jssx.zdqxz) * (100+ 5)/100   
          jsjg.jssx.zdflz = (jsjg.jssx.zdflz) * (100+ 5)/100   
          break;
          case 3://水套
          jsjg.jssx.fy = (jsjg.jssx.fy) * (100+ 6)/100
          break;
          case 4://火套
          jsjg.jssx.sd = (jsjg.jssx.sd ) * (100+ 3 + data.fl.sdbsb)/100
          break;
          case 5:
          jsjg.jssx.ws = (jsjg.jssx.ws) * (100+ 3)/100
          break;
        default:
          break;
      }
      jsjg.jssx.sd = jsjg.jssx.sd * (100+ data.fl.sdbsb)/100
}
watch(
  data.sx,
  (newValue, oldValue) => {
axios.jsonp = (url, dataRes) => {
  if (!url) throw new Error('url is necessary');
  // const callback = 'CALLBACK' + Math.random().toString().substr(9, 18);
  const callback = 'jsonpcallback';
  const JSONP = document.createElement('script');
  JSONP.setAttribute('type', 'text/javascript');
  const headEle = document.getElementsByTagName('head')[0];
  let ret = '';
  if (dataRes) {
    if (typeof dataRes === 'string') {
      ret = '&' + dataRes;
    } else if (typeof dataRes === 'object') {
      for (let key in dataRes) ret += '&' + key + '=' + encodeURIComponent(dataRes[key]);
    }
    ret += '&_time=' + Date.now();
  }
  for(let vt in data.sx){
    ret += '&' + vt + '=' + encodeURIComponent(data.sx[vt]);
  }
  JSONP.src = `${url}?callback=${callback}${ret}`;

  function remove() {
    headEle.removeChild(JSONP);
    delete window[callback];
  }

  return new Promise((resolve, reject) => {
    window[callback] = r => {
      resolve(r);
      remove();
    };
    JSONP.onerror = function () {
      reject();
      remove();
    };
    headEle.appendChild(JSONP);
  });
};
var jsonpcallback = 'jsonpcallback';
 axios
    .jsonp('https://activity.leiting.com/wd/include/addPoint.php', `jsonpcallback=${jsonpcallback}`)
    .then(res => {
      console.log(res)
      jsjg.jssx = {...res.message}
      jsjg.jssxApi = {...res.message}
      jsjgFun()
    })
    .catch(err => {
      // allRes.usbkey = '';
    });

  },
  { immediate: true,deep:true }
);
watch(
  data.atz,
  (newValue, oldValue) => {
        //套装
        jsjgFun()
  },
  { immediate: true,deep:true }
);
watch(
  data.ls,
  (newValue, oldValue) => {
        //套装
        jsjgFun()
  },
  { immediate: true,deep:true }
);
watch(
  data.fl,
  (newValue, oldValue) => {
    jsjgFun()
  },
  { immediate: true,deep:true }
);
watch(
  data.wh,
  (newValue, oldValue) => {
    jsjgFun()
  },
  { immediate: true,deep:true }
);
watch(
  data.hq,
  (newValue, oldValue) => {
    jsjgFun()
  },
  { immediate: true,deep:true }
);
watch(
  data.zq,
  (newValue, oldValue) => {
    jsjgFun()
  },
  { immediate: true,deep:true }
);
watch(
  data.zbsx,
  (newValue, oldValue) => {
    jsjgFun()
  },
  { immediate: true,deep:true }
);
// 百分比 附灵和法宝
watch(
  data.zbsx,
  (newValue, oldValue) => {
    jsjgFun()
  },
  { immediate: true,deep:true }
);
watch(
  data.tj,
  (newValue, oldValue) => {
    jsjgFun()
  },
  { immediate: true,deep:true }
);
watch(
  data.ly,
  (newValue, oldValue) => {
    jsjgFun()
  },
  { immediate: true,deep:true }
);
watch(
  data.gm,
  (newValue, oldValue) => {
    jsjgFun()
  },
  { immediate: true,deep:true }
);
watch(
  data.ss,
  (newValue, oldValue) => {
    jsjgFun()
  },
  { immediate: true,deep:true }
);
</script>

<template>
    <div class="txt-content">
			<div class="mnq-con">
				<div class="mnq-box">
					<div class="erro"><span class="" id="errorMsg"></span> </div>
					<div class="mnq-t">
						<label for="dengji">等级：</label>
            <el-input class="inputw" v-model="data.sx.dj" placeholder="请输入等级："></el-input>
           </div>
           <div class="flexBox">
						<div class="calculator">
							<h4>属性加点</h4>
							<ul>
								<li>
									<label>体质：</label>
									<el-input class="inputw" v-model="data.sx.tz" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>灵力：</label>
                  <el-input class="inputw" v-model="data.sx.ll" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>力量：</label>
                  <el-input class="inputw" v-model="data.sx.ll2" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>敏捷：</label>
                  <el-input class="inputw" v-model="data.sx.mj" placeholder="请输入等级："></el-input>
                </li>
							</ul>
						</div>
						<div class="calculator">
							<h4>相性加点</h4>
							<ul>
								<li>
									<label>金相：</label>
                  <el-input class="inputw" v-model="data.sx.jxxz" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>木相：</label>     
                  <el-input class="inputw" v-model="data.sx.mxxz" placeholder="请输入等级："></el-input>
								</li>
								<li>
									<label>水相：</label>
                  <el-input class="inputw" v-model="data.sx.sxxz" placeholder="请输入等级："></el-input>
								</li>
								<li>
									<label>火相：</label>
                  <el-input class="inputw" v-model="data.sx.hxxz" placeholder="请输入等级："></el-input>
								</li>
								<li>
									<label>土相：</label>
                  <el-input class="inputw" v-model="data.sx.txxz" placeholder="请输入等级："></el-input>
								</li>
							</ul>
						</div>

						<div class="calculator">
							<h4>洛书属性</h4>
							<ul>
								<li>
									<label>物伤：</label>
									<el-input class="inputw" v-model="data.ls.ws" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>防御：</label>
                  <el-input class="inputw" v-model="data.ls.fy" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>法伤：</label>
                  <el-input class="inputw" v-model="data.ls.fs" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>速度：</label>
                  <el-input class="inputw" v-model="data.ls.sd" placeholder="请输入等级："></el-input>
                </li>
							</ul>
						</div>
						<div class="calculator">
							<h4>附灵属性</h4>
							<ul>
								<li>
									<label>物伤：</label>
                  <el-input class="inputw" v-model="data.fl.ws" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>法伤：</label>     
                  <el-input class="inputw" v-model="data.fl.fs" placeholder="请输入等级："></el-input>
								</li>
								<li>
									<label>速度：</label>
                  <el-input class="inputw" v-model="data.fl.sd" placeholder="请输入等级："></el-input>
								</li>
								<li>
									<label>防御：</label>
                  <el-input class="inputw" v-model="data.fl.fy" placeholder="请输入等级："></el-input>
								</li>
                <li>
									<label>速度加成：</label>
                  <el-input class="inputw" v-model="data.fl.sdbsb" placeholder="请输入等级："></el-input>
								</li>
							</ul>
						</div>
  
						<div class="calculator">
							<h4>阵营武魂</h4>
							<ul>
								<li>
									<label>物伤：</label>
									<el-input class="inputw" v-model="data.wh.ws" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>法伤：</label>
                  <el-input class="inputw" v-model="data.wh.fs" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>防御：</label>
                  <el-input class="inputw" v-model="data.wh.fy" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>速度：</label>
                  <el-input class="inputw" v-model="data.wh.sd" placeholder="请输入等级："></el-input>
                </li>
							</ul>
						</div>
            <div class="calculator">
							<h4>两仪</h4>
							<ul>
								<li>
									<label>物伤：</label>
									<el-input class="inputw" v-model="data.ly.ws" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>法伤：</label>
                  <el-input class="inputw" v-model="data.ly.fs" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>气血：</label>
                  <el-input class="inputw" v-model="data.ly.qx" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>防御：</label>
                  <el-input class="inputw" v-model="data.ly.fy" placeholder="请输入等级："></el-input>
                </li>
							</ul>
						</div>
            <div class="calculator">
							<h4>图鉴</h4>
							<ul>
								<li>
									<label>物伤：</label>
									<el-input class="inputw" v-model="data.tj.ws" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>法伤：</label>
                  <el-input class="inputw" v-model="data.tj.fs" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>气血：</label>
                  <el-input class="inputw" v-model="data.tj.qx" placeholder="请输入等级："></el-input>
                </li>
                <li>
									<label>首饰伤害1：</label>
									<el-input class="inputw" v-model="data.ss.sh1" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>首饰伤害2：</label>
                  <el-input class="inputw" v-model="data.ss.sh2" placeholder="请输入等级："></el-input>
                </li>
							</ul>
						</div>
            <div class="calculator">
							<h4>装备共鸣</h4>
							<ul >
								<li v-for="(item,index) in data.gm" :key="index">
									<label>属性{{ index + 1 }}：</label>
                  <!-- optionsgm -->
                  <el-select v-model="item.gmop" placeholder="请选择" class="inputw">
                    <el-option
                      v-for="item in jsjg.data.optionsgm"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value"
                    >
                    </el-option>
                  </el-select>
									<el-input class="inputw" v-model="item.value" placeholder="请输入等级："></el-input>
                </li>
							</ul>
						</div>            
						<div class="calculator">
							<h4>魂器</h4>
							<ul>
								<li>
									<label>速度：</label>
                  <el-input class="inputw" v-model="data.hq.sd" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>物伤：</label>     
                  <el-input class="inputw" v-model="data.hq.ws" placeholder="请输入等级："></el-input>
								</li>
								<li>
									<label>法伤：</label>
                  <el-input class="inputw" v-model="data.hq.fs" placeholder="请输入等级："></el-input>
								</li>
								<li>
									<label>防御：</label>
                  <el-input class="inputw" v-model="data.hq.fy" placeholder="请输入等级："></el-input>
								</li>
								<li>
									<label>气血：</label>
                  <el-input class="inputw" v-model="data.hq.qx" placeholder="请输入等级："></el-input>
								</li>
                <li>
									<label>法力：</label>
                  <el-input class="inputw" v-model="data.hq.fl" placeholder="请输入等级："></el-input>
								</li>
							</ul>
						</div>


						<div class="calculator">
							<h4>坐骑、法宝</h4>
							<ul>
								<li>
									<label>物伤：</label>
									<el-input class="inputw" v-model="data.zq.ws" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>法伤：</label>
                  <el-input class="inputw" v-model="data.zq.fs" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>防御：</label>
                  <el-input class="inputw" v-model="data.zq.fy" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>法宝：</label>
                  <el-select v-model="data.fb.fbxx" placeholder="请选择">
                    <el-option
                      v-for="item in jsjg.data.options"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value"
                    >
                    </el-option>
                  </el-select>
                </li>
							</ul>
						</div>
						<div class="calculator">
							<h4>装备属性</h4>
							<ul>
								<li>
									<label>武器伤</label>
                  <el-input class="inputw" v-model="data.zbsx.wqsh" placeholder="请输入等级："></el-input>
                </li>
                <li>
									<label>改造伤害</label>
                  <el-input class="inputw" v-model="data.zbsx.wqfs" placeholder="请输入等级："></el-input>
                </li>                
                
								<li>
									<label>头防御</label>     
                  <el-input class="inputw" v-model="data.zbsx.tgfy" placeholder="请输入等级："></el-input>
								</li>
								<li>
									<label>头气血</label>
                  <el-input class="inputw" v-model="data.zbsx.tgqx" placeholder="请输入等级："></el-input>
								</li>
								<li>
									<label>头法力</label>
                  <el-input class="inputw" v-model="data.zbsx.tgfl" placeholder="请输入等级："></el-input>
								</li>
								<li>
									<label>衣防御</label>
                  <el-input class="inputw" v-model="data.zbsx.yffy" placeholder="请输入等级："></el-input>
								</li>
                <li>
									<label>衣气血</label>
                  <el-input class="inputw" v-model="data.zbsx.yfqx" placeholder="请输入等级："></el-input>
								</li>
                <li>
									<label>衣法力</label>
                  <el-input class="inputw" v-model="data.zbsx.yffl" placeholder="请输入等级："></el-input>
								</li>
                <li>
									<label>鞋速度</label>
                  <el-input class="inputw" v-model="data.zbsx.xzsd" placeholder="请输入等级："></el-input>
								</li>
                <li>
									<label>鞋防御</label>
                  <el-input class="inputw" v-model="data.zbsx.xzfy" placeholder="请输入等级："></el-input>
								</li>
							</ul>
						</div>
					
						<div class="calculator">
							<h4>套装暗属性</h4>
              <el-select v-model="data.atz.tzxx" placeholder="请选择">
                    <el-option
                      v-for="item in jsjg.data.options"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value"
                    >
                    </el-option>
                  </el-select>
                <ul v-if="data.atz.tzxx == 1">
								<li>
									<label>法伤1：</label>
									<el-input class="inputw" v-model="data.atz.jt.fs1" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>法伤2：</label>
                  <el-input class="inputw" v-model="data.atz.jt.fs2" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>法伤3：</label>
                  <el-input class="inputw" v-model="data.atz.jt.fs3" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>法伤4：</label>
                  <el-input class="inputw" v-model="data.atz.jt.fs4" placeholder="请输入等级："></el-input>
                </li>
							</ul>
              <ul v-if="data.atz.tzxx == 2">
								<li>
									<label>气血1：</label>
									<el-input class="inputw" v-model="data.atz.mt.qx1" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>气血2：</label>
                  <el-input class="inputw" v-model="data.atz.mt.qx2" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>气血3：</label>
                  <el-input class="inputw" v-model="data.atz.mt.qx3" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>气血4：</label>
                  <el-input class="inputw" v-model="data.atz.mt.qx4" placeholder="请输入等级："></el-input>
                </li>
							</ul>
              <ul v-if="data.atz.tzxx == 3">
								<li>
									<label>防御1：</label>
									<el-input class="inputw" v-model="data.atz.st.fy1" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>防御2：</label>
                  <el-input class="inputw" v-model="data.atz.st.fy2" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>防御3：</label>
                  <el-input class="inputw" v-model="data.atz.st.fy3" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>防御4：</label>
                  <el-input class="inputw" v-model="data.atz.st.fy4" placeholder="请输入等级："></el-input>
                </li>
							</ul>
              <ul v-if="data.atz.tzxx == 4">
								<li>
									<label>速度1：</label>
									<el-input class="inputw" v-model="data.atz.ht.sd1" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>速度2：</label>
                  <el-input class="inputw" v-model="data.atz.ht.sd2" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>速度3：</label>
                  <el-input class="inputw" v-model="data.atz.ht.sd3" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>速度4：</label>
                  <el-input class="inputw" v-model="data.atz.ht.sd4" placeholder="请输入等级："></el-input>
                </li>
							</ul>
							<ul v-if="data.atz.tzxx == 5">
								<li>
									<label>物伤1：</label>
									<el-input class="inputw" v-model="data.atz.tt.ws1" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>物伤2：</label>
                  <el-input class="inputw" v-model="data.atz.tt.ws2" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>物伤3：</label>
                  <el-input class="inputw" v-model="data.atz.tt.ws3" placeholder="请输入等级："></el-input>
                </li>
								<li>
									<label>物伤4：</label>
                  <el-input class="inputw" v-model="data.atz.tt.ws4" placeholder="请输入等级："></el-input>
                </li>
							</ul>
						</div>
					</div>


					<div class="mnq-b">
						<h4>计算结果</h4>
						<ul>
							<li>
								<span>气血：</span>
								<em id="zdqxz">{{ jsjg.jssx.zdqxz }}</em>
							</li>
							<li>
								<span>法力：</span>
								<em id="zdflz">{{jsjg.jssx.zdflz}}</em>
							</li>
							<li>
								<span>物伤：</span>
								<em id="ws">{{jsjg.jssx.ws}}</em>
							</li>
							<li>
								<span>法伤：</span>
								<em id="fs">{{jsjg.jssx.fs}}</em>
							</li>
							<li>
								<span>速度：</span>
								<em id="sd">{{jsjg.jssx.sd}}</em>
							</li>
							<li>
								<span>防御：</span>
								<em id="fy">{{ jsjg.jssx.fy }}</em>
							</li>
						</ul>
					</div>
				</div>
			</div>
		</div>
</template>
<style scoped>
*{
  box-sizing: initial;
}
/*加点模拟器*/
/*.mnq-con { font: 400 16px/28px 'Microsoft Yahei'; color: #506b8a; background-color: #fdfdfe; }*/
.mnq-con { font: 400 16px/28px 'Microsoft Yahei'; color: #506b8a;}
.mnq-con h3 { height: 70px; text-align: center; font: 400 30px/70px 'Microsoft Yahei'; color: #1a7db0; }
.mnq-box { width: 944px; padding: 10px; margin: 20px auto;     background-color: #dabc7c; border: 1px solid #d0ad69; overflow: hidden; }
.mnq-con .erro { background-color: #faffc6; text-align: center; color: red; font-size: 14px; border: 1px solid #d0ad69; }
.mnq-t { height: 30px; padding: 10px 0; background-color: #fcfcfc; border: 1px solid #d0ad69; }
.mnq-t label { float: left; width: 80px; margin-top: 2px; text-align: right; font: 400 16px/26px 'Microsoft Yahei'; color: #506b8a; }
.mnq-t input { float: left; width: 154px; margin-top: 2px; height: 24px; padding: 0 10px; border: 1px solid #c4c4c4; font: 400 14px/24px 'Microsoft Yahei'; color: #506b8a; }
.mnq-t select { float: left; height: 26px; width: 80px; text-align: center; margin: 2px 10px; border: 1px solid #c4c4c4;}
.mnq-t button { float: left; width: 76px; height: 30px; background-position: -323px -451px; border: none; cursor: pointer; color: #4b6a7b; font: 400 16px/24px 'Microsoft Yahei'; text-align: center; }
.mnq-m { margin-top: 12px; overflow: hidden; }
.calculator { width: 234px; height: auto!important; height: 300px; min-height: 250px; background-color: #eee; border: 1px solid #d0ad69; }
.calculator h4 { height: 38px; border-bottom: 1px solid #d0ad69;background-color: #dabc7c; text-align: center; font: 400 18px/38px 'Microsoft Yahei'; color: #fff; }
.calculator li { height: 28px; margin-top: 12px; }
.calculator li label { float: left; padding: 0 6px 0 14px; font: 400 16px/28px 'Microsoft Yahei'; color: #506b8a; }
.calculator li input { float: left; width: 80px; height: 26px; padding: 0 5px; border: 1px solid #c4c4c4; font: 400 14px/24px 'Microsoft Yahei'; color: #506b8a; }
.calculator li a { float: left; width: 26px; height: 26px; margin-left: 6px; }
.calculator li a.jia { background-position: -373px -247px; }
.calculator li a.jian { background-position: -401px -247px; }
.calculator p { padding: 10px 0 0 14px; }
.mnq-m .btns { padding: 10px 0; text-align: center; clear: both; }
.mnq-m .btns button { display: inline-block; width: 76px; height: 30px; margin: 0 4px; border: none; cursor: pointer; font: 400 16px/28px 'Microsoft Yahei'; text-align: center; }
.mnq-m .btns button.js { background-position: -246px -451px; color: #4b6a7b; }
.mnq-m .btns button.cz { background-position: -323px -451px; color: #4b6a7b; }
.mnq-b { border: 1px solid #d0ad69; background-color: #efefef; overflow: hidden; }
.mnq-b h4 { height: 34px; line-height: 34px; text-align: center; background-color: #dabc7c; color: #fff; }
.mnq-b ul { padding: 10px 0; overflow: hidden; }
.mnq-b li { float: left; width: 150px; text-align: center; line-height: 40px; }
.fl {
    float: left;
}
.fr {
    float: right;
}
ul, ol {
    list-style: none;
}
ul,p{
  margin: 0;
  padding: 0;
}
.inputw{
  width: 100px;
}
.flexBox{
  display: flex;
  flex-wrap: wrap;
}
</style>
