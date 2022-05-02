<template>
  <div class="hello">
    <h1>{{ message }}</h1>
    <h2>{{ !showValue || words[message] }}</h2>
  </div>
  <div style="height: 100px">
    <p>
      <strong>{{ firstLetter }}: {{ map1.get(firstLetter) }}</strong>
    </p>
    <p>
      <strong>{{ secondLetter }}: {{ map2.get(secondLetter) }}</strong>
    </p>
  </div>
  <div class="footer">
    <h1>不会</h1>
    <h1>会</h1>
  </div>
</template>

<script setup>
import { onMounted, ref, computed } from "vue";
let message = ref("hello");
const words = {};
let showValue = ref(false);
const map1 = new Map();
const map2 = new Map();
const firstLetter = computed(() => message.value.substr(0, 1));
const secondLetter = computed(() => message.value.substr(1, 1));
function handlerStr(str) {
  let arr = str.match(/[A-Z]+[^A-Z]+/g);
  for (let i = 0; i < arr.length; i++) {
    arr[i] = arr[i].replace(/\n/g, "");
    const key = arr[i].match(/[A-Z]+/)[0];
    const value = arr[i].match(/[^A-Z]+/)[0];
    words[key] = value;
    if (key.length > 1) {
      const firstLetter = key.substr(0, 1);
      const secondLetter = key.substr(1, 1);
      const firstChar = value.substr(0, 1);
      const secondChar = value.substr(1, 1);
      if (!map1.get(firstLetter)) {
        map1.set(firstLetter, new Set().add(firstChar));
      } else {
        map1.get(firstLetter).add(firstChar);
      }
      if (!map2.get(secondLetter)) {
        map2.set(secondLetter, new Set().add(secondChar));
      } else {
        map2.get(secondLetter).add(secondChar);
      }
    }
  }
}
// handler map

handlerStr(`A圣诞帽ABanglaAC足球队AD钙奶AEaoeAF长颈鹿AG银子AH爱河AI机器人AJ鞋AK枪AL阿狸AM电动车AN蚂蚁AO奥特曼AP苹果AQ安琪拉AR眼镜AS阿尔卑斯棒棒糖ATatmAWawmAX爱心AY奥运五环AZ安卓手机

B宝宝BA靶子BC奔驰BD板凳BE蜜蜂BF蝙蝠BG包裹BH壁虎BI匕首BJ芭蕉扇BK贝壳BL菠萝BM宝马BN豹女BO波BP鞭炮BQ棒球BR冰人BS宝石BT白糖BW冰屋BX冰箱BY白云BZ巴掌

C月亮CA黑板擦CB翅膀CD沙漏CE玩具车CF火线CG唇膏CH彩虹CI磁铁CJ茶几CK车筐CL车轮CM草莓CN纯奶CO葱CP川普CQ重庆火锅CR超人CS游戏CT抽屉CW刺猬CX沉香CY苍蝇CZ锤子

D弓箭DA蛋DB大巴DC电池DE脚蹬DF豆腐DG蛋糕DH电话DI笛子DJ大疆无人机DK短裤DL地雷DM呆毛DN电暖DO哆啦a梦DP灯泡DQ地球仪DR钻戒DS电视DT电梯DW大碗DX大象DY电源DZ电钻

E视力表EA足球EB2b铅笔EC二传ED耳朵EF耳返EG二锅头EH耳环EI钉耙EJ耳机EK尔康EL恶狼EM鹅毛EN工程师小强EO鹅蛋EP猪耳片EQ皇子eqER二人转ES耳勺ET外星人EW二维码EX安全出口EY鳄鱼EZez

F拐杖FA头发FB方便面FC手柄FD飞碟FE飞蛾FG反光镜FH凤凰FI火FJ飞机FK方块FL风铃FM坟墓FN风女FO佛FP飞盘FQ番茄FR腐乳FS风扇FT斧头FW蜂窝FX方向盘FY枫叶FZ风筝

G婴儿车GA盖子GB胳膊GC锅铲GD果冻GE鸽子GF干粉灭火器GH国徽GI礼物GJ骨架GK钢盔GL古鹿GM老干妈GN公牛GO狗GP光盘GQ钢琴GR骨肉相连GS怪兽GT骨头GW狗窝GX光线GY观音GZ古筝

H跨栏HA蛤蟆HB红包HC火车HD蝴蝶HE鹤HF盒饭HG黄瓜HI海HJ火炬HK黑客HL葫芦HM海绵宝宝HN红牛HO猴HP花盆HQ花圈HR黑人HS护士HT核桃HW虎王HX海星HY后裔HZ耗子

I筷子IA埃菲尔铁塔IB订书机IC挂钩ID身份证IE浏览器IF衣服IG电竞IH滑雪板IJ鱼竿IK竹蜻蜓IL门球杆IM山IN弹簧IO门把手IPiphoneIQ爱因斯坦IR红外线IS蛇IT钉子IW手表IX折叠凳IY弹弓IZ马桶

J雨伞JA咖啡JB胶布JC卷尺JD剪刀JE毽JF卷发棒JG济公JH菊花JI鸡JK监控JL锦鲤JM橘猫JN胶囊JO脚JP键盘JQ机枪JR杰瑞JS僵尸JT吉他JW九尾JX巨蟹JY加油站JZ饺子

K国王KA卡车KB快板KC裤衩KD矿灯KE龟壳KF肯德基KG空格KH口红KIkittyKJ铠甲KL可乐KM孔明灯KN柯南KOlongKP开瓶器KQ孔雀KR烤肉KS狂鼠KT空调KW匡威KX烤箱KY烤鸭KZ口罩

L吸管LA长号LB喇叭LC绿茶LD漏斗LEled灯LF凉粉LG乐高LH轮滑LI梨LJ垃圾桶LK篮筐LM凉面LN露娜LO莲藕LP罗盘LQ篮球LR狼人LS螺丝LT水龙头LW龙王LX龙虾LY轮椅LZ蜡烛

M桥MA马MB面包MC我的世界MD毛肚ME木耳MF米饭MG蘑菇MH棉花MI小米插排MJ麻将MK麦克风ML玛丽MN母牛MO馍MPmp4MQ煤气罐MR美容仪MS盲僧MT木头MW马尾MX毛线MY绵羊MZ帽子

N门NA唢呐NB尿布NC奶茶ND脑袋NE内衣(女)NF农夫NG南瓜NH暖壶NI水泥NJ女警NK纽扣NL奶酪NM纳米cpuNOno牌子NP尿盆NQ暖气片NR脑仁NS掌机NT奶糖NW鸟窝NX女靴NY奶油NZ闹钟

O游泳圈OAc4OB兔子OC手环OD子弹OE梳子OF藕粉OG奖杯OH单杠OI气球OJ铁环OKok手势OL奥利奥OM欧姆电阻ON洋葱OP藕片OQ望远镜OR橘子OS系统盘OT蛋挞OW猫头鹰OX弹夹OY欧阳锋OZ欧洲建筑

P口哨PApapi酱PB皮鞭PC瓢虫PD皮蛋PE屁股PF屏风PG排骨PH胖虎PI猪PJ啤酒PK扑克PL盘龙PM泡面PN乒乓球PO坡PQ喷泉PR飘柔PS披萨PT葡萄PW喷雾PX皮鞋PY皮影PZ盆子

Qqq糖QA恰恰瓜子QB钱包QC球场QD强盗QE企鹅QF鸵鸟QG气管QH清华QI棋QJ青椒QK秋裤QL麒麟QM球门QN琴女QO悠悠球QP气泡QR奇瑞qqQS骑士QT打气筒QW七娃QX千玺QY汽油QZ橘子

R火箭RA太阳神RB热巴RC羊肉串RD溶洞RE蕾姆RF乳房RG忍龟RH如花RI太阳RJ日记RK热裤RL日历RM人民币RN雨RO生肉RP软盘RQ容器RS人参RT肉体RW瑞文RX人心RY人眼RZ人字拖

S八卦SA洒水车SB鼠标SC赛车SDsd娃娃SE大头儿子SF沙发SG试管SH屎SI丝SJ树精SK萨克斯SL锁链SM水母SN酸奶SO肥皂SP算盘SQ手枪SR酥肉ST石头SW丝袜SX神仙SY手游SZ勺子

Tt台TA塔TB拖布TC天秤TD土豆TE天鹅TFtfboysTG跳高绳TH桃花TI梯子TJ太监TK坦克TL铁路TM汤姆TN鸵鸟TO桶TP天蓬TQ铁锹TR土壤TS天使TW套娃TX剃须刀TY汤圆TZ听诊器

W锯齿WA袜子WB尾巴WC公厕WD豌豆射手WE胃WF汪峰WG乌龟WH网红WIwifi路由器WJ围巾WK悟空WL魍魉WM外卖WN万能充WO蜗牛WP卧铺WQ温泉WR微软方块电脑WS卫生纸WT舞台WX蚊香WY乌鸦WZ蚊子

X交通牌XA夹子XB雪碧XC相册XD鞋垫XE蝎XF校服XG西瓜XH小号XI吸尘器XJ香蕉XK显卡XL香炉XM熊猫XN犀牛XO蝴蝶交配XP橡皮XQ雪球XR仙人掌XS萧山地铁站XT鞋套XW显微镜XY香烟XZ洗澡缸

Y树枝YA鸭子YB元宝YC牙齿YD熨斗YE叶子YF音符YG牙膏YH烟花YI衣架YJ眼镜YK遥控YL哑铃YM羽毛球YN椰奶YO柚子YP优盘YQ牙签YR鱼人YS钥匙YT油条YW鹦鹉YX音响YZ眼罩

Z桌子ZA杂质ZB嘴巴ZC直尺ZD字典ZE沼泽ZF张飞ZG杂格ZH蜘蛛ZI水枪ZJ指甲刀ZK钟馗ZL蟑螂ZM芝麻球ZN樟脑球ZO粽子ZP帐篷ZQ桌球ZR孜然ZS钻石ZT枕头ZW指纹ZX猪血ZY章鱼哥`);
const codeArr = Object.keys(words);
function getRandomCode() {
  let code = "";
  while (code === message.value || !code) {
    code = codeArr[parseInt(Math.random() * codeArr.length)];
  }
  return code;
}
message.value = getRandomCode();
onMounted(() => {
  document.addEventListener(
    "click",
    function (e) {
      // console.log(e);
      const leftInstance = e.screenX;
      const screenWidth = window.innerWidth;
      if (leftInstance < screenWidth / 2) {
        showValue.value = true;
      } else {
        if (showValue.value == true) {
          showValue.value = false;
          message.value = getRandomCode();
        } else {
          showValue.value = true;
          setTimeout(() => {
            showValue.value = false;
            message.value = getRandomCode();
          }, 500);
        }
      }
    },
    false
  );
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.footer {
  display: flex;
  justify-content: space-around;
  user-select: none;
}
</style>
