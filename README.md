
## 基础组件


**常用组件**


* Text：显示文本内容
* Image：显示图片
* Button：显示一个按钮
* Column： 纵向布局
* Row：横向布局
* List：列表


各组件的用法



```
Text("文本组件")
  .fontColor(Theme.Color.textPrimary)
  .fontWeight(FontWeight.Medium)
  .fontSize(20)

Image($r('app.media.banner'))
        .objectFit(ImageFit.Fill)
        .width('100%')
        .height(100)
        .opacity(191)
 
Button("按钮").onClick((event) => {
    //按钮点击事件
})

Column() {
  Text("文本1")
  .fontColor(Theme.Color.textPrimary)
  .fontWeight(FontWeight.Medium)
  .fontSize(20)
  Text("文本2")
  .fontColor(Theme.Color.textPrimary)
  .fontWeight(FontWeight.Medium)
  .fontSize(20)
  Text("文本3")
  .fontColor(Theme.Color.textPrimary)
  .fontWeight(FontWeight.Medium)
  .fontSize(20)
}.width('100%')

Row() {
  Text("文本1")
  .fontColor(Theme.Color.textPrimary)
  .fontWeight(FontWeight.Medium)
  .fontSize(20)
  Text("文本2")
  .fontColor(Theme.Color.textPrimary)
  .fontWeight(FontWeight.Medium)
  .fontSize(20)
  Text("文本3")
  .fontColor(Theme.Color.textPrimary)
  .fontWeight(FontWeight.Medium)
  .fontSize(20)
}.width('100%')

List() {
  ForEach(this.items, (item: string, index) => {
    ListItem(){
      Text(item).padding(10).width('100%')
    }
  })
}


```

**Grid**
用于展示网格列表



```
Grid(){
  GridItem(){
    Text("item1").padding(10)
  }
  GridItem(){
    Text("item2").padding(10)
  }
  GridItem(){
    Text("item12").padding(10)
  }
}.columnsTemplate("1f 1f 1f")  //列分布规则
.rowsTemplate("1f 1f 1f")		//行分布规则

```

Grid的夸行/列设置


在创建Grid时可以传入GridLayoutOptions来实现网格所占行/列数


1. regularSize，类型为\[number，number]，用来定义一般规则的Item所占的行/列数
2. irregularIndexes，类型为number\[]，用来指定不遵循一般规则的Item索引。
3. onGetIrregularSizeByIndex，类型(index: number) \=\> \[number, number]的回调函数，irregularIndexes中配置的索引会回调到此函数，返回结果表示该Item所占用的行/列数，\[所占行数，所占列数]



```
@Preview
@Component
struct GridList {
  item: Array = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
  layoutOption: GridLayoutOptions = {
    regularSize: [1, 1],  //一般规则的item占用1行1列
    irregularIndexes: [0, 5], //非一般规则item的索引
    onGetIrregularSizeByIndex: (index: number) => {
      if (index == 0) {
        return [1, 2]  //0索引处的item占用1行2列
      } else {
        return [2, 1]  //5索引处的item占用2行1列
      }
    }
  }

  build() {
    Grid(null, this.layoutOption) {
      ForEach(this.item, (item: number, index) => {
        GridItem() {
          Stack() {
            Text(item + "").textAlign(TextAlign.Center)
          }.width('100%').height(index == 5 ? 105 : 50)
        }.backgroundColor(Color.Pink)
      }, (item: number, index) => {
        return item + ""
      })
    }
    .columnsTemplate('1fr 1fr')
    .rowsGap(5)
    .columnsGap(5)
    .width('100%')
    .height('100%')
  }
}

```

![](https://img2024.cnblogs.com/blog/108333/202412/108333-20241209095427947-494648619.png)


**WaterFlow**
瀑布流



```
WaterFlow() {
  FlowItem() {
     Text("item1").padding(10)
  }
  FlowItem() {
   Text("item2").padding(10)
  }
}

```

瀑布流夸行/列设置


使用瀑布流的分组信息（WaterFlowSections）可以设置item不同列数的混合布局。
WaterFlowSections 中可以设置多个分组信息，每个分组信息包含如下内容：
● itemsCount 分组中的item数量
● crossCount 瀑布流的行/列数
● columnsGap/rowsGap：列/行间距
● margin 分组的外边距



```
@Preview
@Component
struct WaterFlowListView {
  items: Array = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
  @State sections: WaterFlowSections = new WaterFlowSections()

  aboutToAppear(): void {
    this.sections.push({
      itemsCount: 1, //分组中的item数量
      crossCount: 1, //分组中的列数
      columnsGap: 8, //列间距
      rowsGap: 8,     //行间距
    })
    this.sections.push({
      itemsCount: 8,
      crossCount: 2,
      columnsGap: 8,
      rowsGap: 8,
      margin:{
        top: 8
      }
    })
    this.sections.push({
      itemsCount: 1,
      crossCount: 1,
      columnsGap: 8,
      rowsGap: 8,
      margin:{
        top: 8
      }
    })
  }

  build() {
    WaterFlow({ sections: this.sections }) {
      ForEach(this.items, (item: number, index) => {
        FlowItem() {
          Stack() {
            Text(item + "").textAlign(TextAlign.Center)
          }.height(index*10 + 30)
        }.backgroundColor(Color.Pink).width('100%')
      }, (item: number, index) => {
        return item + ""
      })
    }
    .width('100%')
    .height('100%')
  }
}

```

![](https://img2024.cnblogs.com/blog/108333/202412/108333-20241209095447106-114924316.png)


## 页面实现


**准备数据**
首先需要准备数据，包括诗词内容，小知识，作者。
json文件内容如下：



```
[
  {
    "title": "满江红·写怀",
    "dynasty": "南宋",
    "author": "岳飞",
    "introduction": "此词上片写作者悲愤中原重陷敌手，痛惜前功尽弃的局面，也表达自己继续努力，争取壮年立功的心愿。\n此词下片运转笔端，抒写词人对于民族敌人的深仇大恨，统一祖国的殷切愿望，忠于朝廷即忠于祖国的赤诚之心。",
    "text": "怒发冲冠，凭阑处、潇潇雨歇。\n抬望眼，仰天长啸，壮怀激烈。\n三十功名尘与土，八千里路云和月。\n莫等闲、白了少年头，空悲切。\n靖康耻，犹未雪。\n臣子恨，何时灭。\n驾长车，踏破贺兰山缺。\n壮志饥餐胡虏肉，笑谈渴饮匈奴血。\n待从头、收拾旧山河，朝天阙。",
    "textAlign": "center",
    "translation": "气得头发竖起，以至于将帽子顶起，登高倚栏杆，一场潇潇细雨刚刚停歇。\n\n抬头望眼四望辽阔一片，仰天长声啸叹，一片报国之心充满心怀。\n\n三十多年来虽已建立一些功名，但如同尘土微不足道，南北转战八千里，经过多少风云人生。\n\n不要虚度年华，花白了少年黑发，只有独自悔恨悲悲切切。\n\n靖康年的奇耻，尚未洗雪。\n\n臣子愤恨，何时才能泯灭。\n\n我要驾着战车向贺兰山进攻，连贺兰山也要踏为平地。\n\n我满怀壮志，打仗饿了就吃敌人的肉，谈笑渴了就喝敌人的鲜血。\n\n我要从头再来，收复旧日河山，朝拜故都京阙。",
    "searchKey": "manjianghong|mjh|mjhxh|song|yuefei|yf"
  },
  {
    "title": "水调歌头·明月几时有",
    "dynasty": "北宋",
    "author": "苏轼",
    "introduction": "词前的小序交待了写词的过程：“丙辰中秋，欢饮达旦，大醉。作此篇，兼怀子由。”苏轼因为与当权的变法者王安石等人政见不同，自求外放，辗转在各地为官。他曾经要求调任到离苏辙较近的地方为官，以求兄弟多多聚会。公元1074年（熙宁七年）苏轼差知密州。到密州后，这一愿望仍无法实现。公元1076年的中秋，皓月当空，银辉遍地，词人与胞弟苏辙分别之后，已七年未得团聚。此刻，词人面对一轮明月，心潮起伏，于是乘酒兴正酣，挥笔写下了这首名篇。",
    "text": "丙辰中秋，欢饮达旦，大醉，作此篇，兼怀子由。\n\n明月几时有？把酒问青天。不知天上宫阙，今夕是何年。我欲乘风归去，又恐琼楼玉宇，高处不胜寒。起舞弄清影，何似在人间。\n\n转朱阁，低绮户，照无眠。不应有恨，何事长向别时圆？人有悲欢离合，月有阴晴圆缺，此事古难全。但愿人长久，千里共婵娟。",
    "textAlign": "left",
    "translation": "丙辰年的中秋节，高兴地喝酒直到第二天早晨，喝到大醉，写了这首词，同时思念弟弟苏辙。\n\n明月从什么时候才开始出现的？我端起酒杯遥问苍天。不知道在天上的宫殿，何年何月。我想要乘御清风回到天上，又恐怕在美玉砌成的楼宇，受不住高耸九天的寒冷。翩翩起舞玩赏着月下清影，哪像是在人间。\n\n月儿转过朱红色的楼阁，低低地挂在雕花的窗户上，照着没有睡意的自己。明月不该对人们有什么怨恨吧，为什么偏在人们离别时才圆呢？人有悲欢离合的变迁，月有阴晴圆缺的转换，这种事自古来难以周全。只希望这世上所有人的亲人能平安健康，即便相隔千里，也能共享这美好的月光。",
    "rectify": {
      "高触不胜寒": "高触不胜寒"
    },
    "searchKey": "shuidiaogetou|mingyuejishiyou|sdgt|myjsy|song|sushi|ss"
  },
  {
    "title": "登高",
    "dynasty": "唐",
    "author": "杜甫",
    "introduction": "此诗作于公元767年（唐代宗大历二年）秋天，杜甫时在夔州。这是他在五十六岁时写下的。一天他独自登上夔州白帝城外的高台，登高临眺，萧瑟的秋江景色，引发了他身世飘零的感慨，渗入了他老病孤愁的悲哀。于是，就有了这首被誉为“七律之冠”的《登高》。",
    "text": "风急天高猿萧哀，渚清沙白鸟飞回；\n无边落木萧萧下，不尽长江滚滚来。\n万里悲秋常作客，百年多病独登台；\n艰难苦恨繁霜鬓，潦倒新停浊酒杯。",
    "textAlign": "center",
    "translation": "风急天高猿猴啼叫显得十分悲哀，水清沙白的河洲上有鸟儿在盘旋。\n无边无际的树木萧萧地飘下落叶，长江滚滚涌来奔腾不息。\n悲对秋景感慨万里漂泊常年为客，一生当中疾病缠身今日独上高台。\n历尽了艰难苦恨白发长满了双鬓，衰颓满心偏又暂停了浇愁的酒杯。",
    "searchKey": "denggao|dg|tang|dufu|dg"
  },
  ……
]

```


```
[
  {
    "title": "唐宋八大家",
    "text": "唐宋八大家，又称为“唐宋散文八大家”，是唐代和宋代八位散文家的合称，分别为唐代韩愈、柳宗元和宋代欧阳修、苏洵、苏轼、苏辙、王安石、曾巩八位。\n其中韩愈、柳宗元是唐代古文运动的领袖，欧阳修、三苏（苏轼、苏辙、苏洵）等四人是宋代古文运动的核心人物，王安石、曾巩是临川文学的代表人物。他们先后掀起的古文革新浪潮，使诗文发展的陈旧面貌焕然一新。\n八大家中苏家父子兄弟有三人，人称“三苏”，分别为苏洵、苏轼、苏辙，又有“一门三学士”之誉。故可用“韩柳欧王曾三苏”概括。"
  },
  {
    "title": "醉八仙",
    "text": "醉八仙，指唐朝嗜酒的八位学者名人，亦称酒中八仙、饮中八仙。《新唐书·李白传》载，李白、贺知章、李适之、汝阳王李琎、崔宗之、苏晋、张旭、焦遂为“酒中八仙人”。杜甫有《饮中八仙歌》。瓷器画面绘饮中八仙，每每于人物之上书以人名，以清朝为多见。\n一仙贺知章：知章骑马似乘船，眼花落井水底眠。\n二仙汝阳王：汝阳三斗始朝天，道逢麴车口流涎，恨不移封向酒泉。\n三仙李适之：左相日兴费万钱，饮如长鲸吸百川，衔杯乐圣称避贤。\n四仙崔宗之：宗之潇洒美少年，举觞白眼望青天，皎如玉树临风前。\n五仙苏晋：苏晋长斋绣佛前，醉中往往爱逃禅。\n六仙李白：李白一斗诗百篇，长安市上酒家眠。天子呼来不上船，自称臣是酒中仙。\n七仙张旭：张旭三杯草圣传，脱帽露顶王公前，挥毫落纸如云烟。\n八仙焦遂：焦遂五斗方卓然，高谈雄辩惊四筵。"
  },
  ……
]

```


```
[
  {
    "name": "苏轼",
    "dynasty": "北宋",
    "introduction": "苏轼（1037年—1101年），字子瞻，又字和仲，号铁冠道人、东坡居士，世称苏东坡、苏仙、坡仙。眉州眉山（今四川省眉山市）人，北宋文学家，书法家、画家，历史治水名人。与父苏洵、弟苏辙三人并称“三苏”。\n嘉祐二年（1057年），参加殿试中乙科，赐进士及第（一说赐进士出身）。嘉祐六年（1061年），参加制科考试，授大理评事、签书凤翔府判官。宋神宗时，曾在杭州、密州、徐州、湖州等地任职。元丰三年（1080年），因“乌台诗案”，被贬为黄州团练副使。宋哲宗即位后，出任兵部尚书、礼部尚书等职，外放治理杭州、颍州、扬州、定州等地。随着新党执政，又被贬惠州、儋州。宋徽宗时，获赦北还，病逝于常州。南宋时期，追赠太师，谥号“文忠”。\n苏轼是北宋中期文坛领袖，在诗、词、文、书、画等方面取得很高成就。其诗题材广阔，清新豪健，善用夸张比喻，独具风格，与黄庭坚并称“苏黄”；其词开豪放一派，与辛弃疾同是豪放派代表，并称“苏辛”；其文著述宏富，纵横恣肆，豪放自如，与欧阳修并称“欧苏”，与韩愈、柳宗元、欧阳修、苏洵、苏辙、王安石、曾巩合称“唐宋八大家”；善书法，与黄庭坚、米芾、蔡襄合称“宋四家”；擅长文人画，尤擅墨竹、怪石、枯木等。作品有《东坡七集》《东坡易传》《东坡乐府》《寒食帖》《潇湘竹石图》《枯木怪石图》等。"
  },
  {
    "name": "杜甫",
    "dynasty": "唐",
    "introduction": "杜甫（712年2月12日—770年），字子美，自号少陵野老，祖籍襄阳（今属湖北），自其曾祖时迁居巩县（今河南巩义西南）。唐代著名现实主义诗人。与李白合称“李杜”“大李杜”，也常被称为“老杜”。\n杜甫自幼好学，知识渊博，颇有政治抱负。唐玄宗开元后期，举进士不第，漫游各地。后寓居长安近十年，未能有所施展，生活贫困，逐渐对当时的社会状况有较深的认识。后靠献赋才得授小官。安史之乱爆发、长安失陷后，他曾被困城中半年，后逃至凤翔，被唐肃宗拜为左拾遗，世称“杜拾遗”。长安收复后，随肃宗还京，又被外放为华州司功参军。期间创作了《登高》《春望》《北征》以及“三吏”“三别”等名作。后弃官移家至成都，一度在剑南节度使严武幕中任参谋，被表授为检校工部员外郎，故世称“杜工部”。晚年携家出蜀，于大历五年（770年）冬在辗转途中逝世，享年五十九岁。\n杜甫善于运用各种诗歌形式，尤长于律诗，风格多样，而以沉郁为主；语言精练，具有高度的表达能力。继承和发展《诗经》以来注重反映社会现实的优良文学传统，成为中国古代诗歌艺术发展的又一高峰，被后人公认为诗歌史上的“集大成者”。他的人格，也被认为是中华民族文人品格的楷模。自晚唐两宋后，杜甫逐渐声名远播，对中国文学和日本文学都产生了深远的影响。后世尊称他为“诗圣”，称其诗为“诗史” 。其传世作品大多集于《杜工部集》。"
  },
  {
    "name": "岳飞",
    "dynasty": "南宋",
    "introduction": "岳飞（1103年3月24日—1142年1月27日），字鹏举，宋朝相州汤阴（今河南汤阴）人，祖籍东昌（今山东聊城），南宋时期抗金名将、军事家、战略家、民族英雄、书法家、诗人，位列南宋“中兴四将”之首。\n岳飞从二十岁起，曾先后四次从军。自建炎二年（1128年）遇宗泽至绍兴十一年（1141年）止，先后参与、指挥大小战斗数百次。金军攻打江南时，独树一帜，力主抗金，收复建康。绍兴四年（1134年），收复襄阳六郡。绍兴六年（1136年），率师北伐，顺利攻取商州、虢州等地。绍兴十年（1140年），完颜宗弼毁盟攻宋，岳飞挥师北伐，两河人民奔走相告，各地义军纷纷响应，夹击金军。岳家军先后收复郑州、洛阳等地，在郾城、颍昌大败金军，进军朱仙镇。宋高宗赵构和宰相秦桧却一意求和，以十二道“金字牌”催令班师。在宋金议和过程中，岳飞遭受秦桧、张俊等人诬陷入狱。1142年1月，以莫须有的罪名，与长子岳云、部将张宪一同遇害。宋孝宗时，平反昭雪，改葬于西湖畔栖霞岭，追谥武穆，后又追谥忠武，封鄂王。 \n岳飞是南宋杰出的统帅，他重视人民抗金力量，缔造了“连结河朔”之谋，主张黄河以北的民间抗金义军和宋军互相配合，以收复失地；治军赏罚分明，纪律严整，又能体恤部属，以身作则，率领的“岳家军”号称“冻死不拆屋，饿死不掳掠”。金军有“撼山易，撼岳家军难”的评语，以示对岳家军的由衷敬佩。\n岳飞的文才同样卓越，其代表词作《满江红·怒发冲冠》是千古传诵的爱国名篇，后人辑有文集传世。"
  },
  ……
}

```

将json文件放置在资源目录 resources/rawfile/ 下载
使用时通过 import 导入



```
import poetryList from 'resources/rawfile/poetry.json';
import tips from 'resources/rawfile/tips.json';

```

根据json数据格式定义数据实体类



```
export interface Poetry {
  uuid?: string
  title: string
  dynasty: string
  author: string
  introduction: string
  text: string
  textAlign: string
  translation: string
  rectify?: object
  searchKey: string
}

export class Tip {
  uuid: string
  title: string
  text: string
}

```

**首页实现**


首页实现效果
![](https://img2024.cnblogs.com/blog/108333/202412/108333-20241209095503806-283600084.png)


首页内容包括一个随机的小知识和诗词列表，因此我们可以使用List，Grid，WaterFlow组件来实现列表展示，这里我们以WaterFlow为例。


**小知识布局**


![](https://img2024.cnblogs.com/blog/108333/202412/108333-20241209095541637-175471419.png)



```
@Component
export struct TipView {
  @Prop tip: Tip
  viewHeight: number = 152
  textFontSize: number = 19
  textController: TextController = new TextController();

  applyTextStyle() {
    let style = TextStyleUtils.buildParagraphIndentStyle(this.tip.text, this.textFontSize * 2)
    this.textController.setStyledString(style)
  }

  build() {
    Stack({
      alignContent: Alignment.TopStart
    }) {
      Image($r('app.media.tip_banner'))
        .objectFit(ImageFit.Fill)
        .width('100%')
        .height(this.viewHeight)
        .opacity(191)
      Column() {
        Text(this.tip.title)
          .fontSize(20)
          .fontWeight(FontWeight.Medium)
        Text(undefined, { controller: this.textController })
          .fontSize(this.textFontSize)
          .lineHeight(26)
          .maxLines(4)
          .textOverflow({
            overflow: TextOverflow.Ellipsis
          })
          .margin({ top: 5 })
          .width("100%")
          .onAppear(() => {
            this.applyTextStyle()
          })
      }
      .alignItems(HorizontalAlign.Start)
      .padding({
        top: 10,
        bottom: 10,
        left: 12,
        right: 12
      })
      .width('100%')
      .constraintSize({ maxHeight: this.viewHeight })
      .backgroundColor(Theme.Color.tipShade)
    }
    .clip(true)
    .borderRadius(10)
  }
}

```

小知识内容涉及多个段落，为了阅读方便，段落开始需要缩进2个汉字，HarmonyOS提供了 ParagraphStyle 类设置段落样式。



```
export class TextStyleUtils {

  static buildParagraphIndentStyle(text: string, offset: number): MutableStyledString {
    let paragraphStyle: ParagraphStyle = new ParagraphStyle({ textIndent: LengthMetrics.vp(offset) });
    let regArray = text.matchAll(/\n/g)
    let styleArray: Array<StyleOptions> = []
    styleArray.push({
      start: 0,
      length: 3,
      styledKey: StyledStringKey.PARAGRAPH_STYLE,
      styledValue: paragraphStyle
    })
    for (let match of regArray) {
      styleArray.push({
        start: match.index,
        length: 3,
        styledKey: StyledStringKey.PARAGRAPH_STYLE,
        styledValue: paragraphStyle
      })
    }
    return new MutableStyledString(text, styleArray)
  }
}

```

**诗词列表项布局**


![](https://img2024.cnblogs.com/blog/108333/202412/108333-20241209095526767-1851634305.png)



```

@Component
struct DataItemView {
  @Prop item: Poetry

  build() {

    Column() {
      Text(this.item.title)
        .fontColor(Theme.Color.textPrimary)
        .fontWeight(FontWeight.Medium)
        .fontSize(20)
      Text(`[${this.item.dynasty}]${this.item.author}`)
        .fontColor(Theme.Color.textSecondary)
        .fontSize(16)
        .margin({ top: 2 })
      Text(this.item.introduction)
        .fontColor(Theme.Color.textSecondary)
        .fontSize(16)
        .margin({ top: 5 })
        .maxLines(3)
        .textOverflow({
          overflow: TextOverflow.Ellipsis
        })
    }
    .alignItems(HorizontalAlign.Start)
    .padding(10)
  }
}

```

**瀑布流列表**


列表第一项是小知识布局，需要夸2列显示，因此列表项分组如下：


1. 小知识布局，数量为1，展示1列
2. 诗词项布局，数量为诗词列表数量，展示2列
3. 底部留白，数量为1，展示1列



```
@Component
struct WaterFlowView {
  @Prop tip: Tip
  @Prop poetryList: LazyDataSource<Poetry>
  @State sections: WaterFlowSections = new WaterFlowSections()

  aboutToAppear(): void {
    this.sections.push({
      itemsCount: 1,
      crossCount: 1,
    })
    this.sections.push({
      itemsCount: this.poetryList.totalCount(),
      crossCount: 2,
      columnsGap: 8,
      rowsGap: 8,
    })
    this.sections.push({
      itemsCount: 1,
      crossCount: 1,
    })
  }

  build() {
    WaterFlow({ sections: this.sections, layoutMode: WaterFlowLayoutMode.SLIDING_WINDOW }) {
      FlowItem() {
        TipView({
          tip: this.tip,
        })
      }
      .padding({ top: 8, bottom: 8 })
      .onClick(() => {
      })

      LazyForEach(this.poetryList,
        (item: Poetry, index) => {
          FlowItem() {
            DataItemView({
              item: item
            })
          }
          .width('100%')
          .borderRadius(10)
          .backgroundColor(Theme.Color.inputEditTextBackground)
          .onClick(() => {
          })
        },
        (item: Poetry, index) => {
          return item.uuid
        }
      )
      FlowItem() {
        Text().height(8).width('100%')
      }
    }
    .width('100%')
    .height('100%')
    .padding({ left: 8, right: 8 })
  }

```

**详情页实现**
详情页实现效果
![](https://img2024.cnblogs.com/blog/108333/202412/108333-20241209095601245-1189724278.png)


诗词正文部分可以根据内容居中或居左展示，【介绍】部分也是需要段落缩进（参考小知识布局）。详情页布局比较简单，不在详述。




---


本文的技术设计和实现都是基于作者工作中的经验总结，如有错误，请留言指正，谢谢。


 本博客参考[Flowercloud 机场订阅加速](https://flowercloud6.com)。转载请注明出处！
