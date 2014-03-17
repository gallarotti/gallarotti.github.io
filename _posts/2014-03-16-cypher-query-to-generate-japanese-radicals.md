---
layout: blog_entry
comments: true
title: Cypher Query to Generate all Japanese Radicals
---
There are probably less than 5 people in the entire world that might be interested in this piece of information, including me, and those people might already have found their own way to achieve the same result. Nonetheless, I am writing this quick blog post, mostly for personal reference.

The Cypher query published at the end of this post is used to reset (i.e. clear) an existing Neo4J database and then recreate the 214 nodes - one for each of the Japanese radicals - needed to bring the project I am working on to a well-know state. The query is run whenever testing has brought the system to an unclean state.

To generate this query, first I got the raw data from the [Wikipedia Page on Kangxi radicals](http://en.wikipedia.org/wiki/Kangxi_radical), which then I massaged to look like this:

```
1|一|||1|one|42
2|丨|||1|line|21
3|丶|||1|dot|10
4|丿|||1|slash|33
5|乙|⺄|乚|1|second|42
6|亅|||1|hook|19
7|二|||2|two|29
8|亠|||2|lid|38
9|人|亻|𠆢|2|man|794
10|儿|||2|legs|52
...
```

Then I wrote the following PERL script, which parses through the text file above and generates the Cypher query:

```perl
#!/usr/bin/perl -w
print "MATCH n-[r]->()\n";
print "DELETE r\n";
print "WITH COUNT(n) AS hack\n";
print "MATCH n\n";
print "DELETE n\n"; 
print "WITH COUNT(n) AS hack\n";
print "CREATE\n";
my $counter = 0;
foreach $line (<STDIN>) {
    chomp($line);              # remove the newline from $line.
	@record = split /\|/, $line;
	if ($counter > 0) {
		print ",\n";
	}
	print "(R$record[0]:Radical{";
	print "index:'$record[0]',";
	print "character:'$record[1]',";
	print "altchar1:'$record[2]',";
	print "altchar2:'$record[3]',";
	print "strokes:'$record[4]',";
	print "meaning:'$record[5]',";
	print "frequency:'$record[6]'";
	print "})";
	$counter++;
}
```

And here is the generated query:

```
MATCH n-[r]->()
DELETE r
WITH COUNT(n) AS hack
MATCH n
DELETE n
WITH COUNT(n) AS hack
CREATE
(R1:Radical{id:'1',char:'一',altchar1:'',altchar2:'',strokes:'1',meaning:'one',freq:'42'}),
(R2:Radical{id:'2',char:'丨',altchar1:'',altchar2:'',strokes:'1',meaning:'line',freq:'21'}),
(R3:Radical{id:'3',char:'丶',altchar1:'',altchar2:'',strokes:'1',meaning:'dot',freq:'10'}),
(R4:Radical{id:'4',char:'丿',altchar1:'',altchar2:'',strokes:'1',meaning:'slash',freq:'33'}),
(R5:Radical{id:'5',char:'乙',altchar1:'⺄',altchar2:'乚',strokes:'1',meaning:'second',freq:'42'}),
(R6:Radical{id:'6',char:'亅',altchar1:'',altchar2:'',strokes:'1',meaning:'hook',freq:'19'}),
(R7:Radical{id:'7',char:'二',altchar1:'',altchar2:'',strokes:'2',meaning:'two',freq:'29'}),
(R8:Radical{id:'8',char:'亠',altchar1:'',altchar2:'',strokes:'2',meaning:'lid',freq:'38'}),
(R9:Radical{id:'9',char:'人',altchar1:'亻',altchar2:'𠆢',strokes:'2',meaning:'man',freq:'794'}),
(R10:Radical{id:'10',char:'儿',altchar1:'',altchar2:'',strokes:'2',meaning:'legs',freq:'52'}),
(R11:Radical{id:'11',char:'入',altchar1:'',altchar2:'',strokes:'2',meaning:'enter',freq:'28'}),
(R12:Radical{id:'12',char:'八',altchar1:'',altchar2:'',strokes:'2',meaning:'eight',freq:'44'}),
(R13:Radical{id:'13',char:'冂',altchar1:'',altchar2:'',strokes:'2',meaning:'down box',freq:'50'}),
(R14:Radical{id:'14',char:'冖',altchar1:'',altchar2:'',strokes:'2',meaning:'cover',freq:'30'}),
(R15:Radical{id:'15',char:'冫',altchar1:'',altchar2:'',strokes:'2',meaning:'ice',freq:'115'}),
(R16:Radical{id:'16',char:'几',altchar1:'',altchar2:'',strokes:'2',meaning:'table',freq:'38'}),
(R17:Radical{id:'17',char:'凵',altchar1:'',altchar2:'',strokes:'2',meaning:'open box',freq:'23'}),
(R18:Radical{id:'18',char:'刀',altchar1:'刂',altchar2:'',strokes:'2',meaning:'knife',freq:'377'}),
(R19:Radical{id:'19',char:'力',altchar1:'',altchar2:'',strokes:'2',meaning:'power',freq:'163'}),
(R20:Radical{id:'20',char:'勹',altchar1:'',altchar2:'',strokes:'2',meaning:'wrap',freq:'64'}),
(R21:Radical{id:'21',char:'匕',altchar1:'',altchar2:'',strokes:'2',meaning:'spoon',freq:'19'}),
(R22:Radical{id:'22',char:'匚',altchar1:'',altchar2:'',strokes:'2',meaning:'right open box',freq:'64'}),
(R23:Radical{id:'23',char:'匸',altchar1:'',altchar2:'',strokes:'2',meaning:'hiding enclosure',freq:'17'}),
(R24:Radical{id:'24',char:'十',altchar1:'',altchar2:'',strokes:'2',meaning:'ten',freq:'55'}),
(R25:Radical{id:'25',char:'卜',altchar1:'',altchar2:'',strokes:'2',meaning:'divination',freq:'45'}),
(R26:Radical{id:'26',char:'卩',altchar1:'㔾',altchar2:'',strokes:'2',meaning:'seal',freq:'40'}),
(R27:Radical{id:'27',char:'厂',altchar1:'',altchar2:'',strokes:'2',meaning:'cliff',freq:'129'}),
(R28:Radical{id:'28',char:'厶',altchar1:'',altchar2:'',strokes:'2',meaning:'private',freq:'40'}),
(R29:Radical{id:'29',char:'又',altchar1:'',altchar2:'',strokes:'2',meaning:'again',freq:'91'}),
(R30:Radical{id:'30',char:'口',altchar1:'',altchar2:'',strokes:'3',meaning:'mouth',freq:'1146'}),
(R31:Radical{id:'31',char:'囗',altchar1:'',altchar2:'',strokes:'3',meaning:'enclosure',freq:'118'}),
(R32:Radical{id:'32',char:'土',altchar1:'',altchar2:'',strokes:'3',meaning:'earth',freq:'580'}),
(R33:Radical{id:'33',char:'士',altchar1:'',altchar2:'',strokes:'3',meaning:'scholar',freq:'24'}),
(R34:Radical{id:'34',char:'夂',altchar1:'',altchar2:'',strokes:'3',meaning:'go',freq:'11'}),
(R35:Radical{id:'35',char:'夊',altchar1:'',altchar2:'',strokes:'3',meaning:'go slowly',freq:'23'}),
(R36:Radical{id:'36',char:'夕',altchar1:'',altchar2:'',strokes:'3',meaning:'evening',freq:'34'}),
(R37:Radical{id:'37',char:'大',altchar1:'',altchar2:'',strokes:'3',meaning:'big',freq:'132'}),
(R38:Radical{id:'38',char:'女',altchar1:'',altchar2:'',strokes:'3',meaning:'woman',freq:'681'}),
(R39:Radical{id:'39',char:'子',altchar1:'',altchar2:'',strokes:'3',meaning:'child',freq:'83'}),
(R40:Radical{id:'40',char:'宀',altchar1:'',altchar2:'',strokes:'3',meaning:'roof',freq:'246'}),
(R41:Radical{id:'41',char:'寸',altchar1:'',altchar2:'',strokes:'3',meaning:'inch',freq:'40'}),
(R42:Radical{id:'42',char:'小',altchar1:'⺌',altchar2:'⺍',strokes:'3',meaning:'small',freq:'41'}),
(R43:Radical{id:'43',char:'尢',altchar1:'尣',altchar2:'',strokes:'3',meaning:'lame',freq:'66'}),
(R44:Radical{id:'44',char:'尸',altchar1:'',altchar2:'',strokes:'3',meaning:'corpse',freq:'148'}),
(R45:Radical{id:'45',char:'屮',altchar1:'',altchar2:'',strokes:'3',meaning:'sprout',freq:'38'}),
(R46:Radical{id:'46',char:'山',altchar1:'',altchar2:'',strokes:'3',meaning:'mountain',freq:'636'}),
(R47:Radical{id:'47',char:'巛',altchar1:'川',altchar2:'巜',strokes:'3',meaning:'river',freq:'26'}),
(R48:Radical{id:'48',char:'工',altchar1:'',altchar2:'',strokes:'3',meaning:'work',freq:'17'}),
(R49:Radical{id:'49',char:'己',altchar1:'',altchar2:'',strokes:'3',meaning:'oneself',freq:'20'}),
(R50:Radical{id:'50',char:'巾',altchar1:'',altchar2:'',strokes:'3',meaning:'turban',freq:'295'}),
(R51:Radical{id:'51',char:'干',altchar1:'',altchar2:'',strokes:'3',meaning:'dry',freq:'9'}),
(R52:Radical{id:'52',char:'幺',altchar1:'',altchar2:'',strokes:'3',meaning:'short thread',freq:'50'}),
(R53:Radical{id:'53',char:'广',altchar1:'',altchar2:'',strokes:'3',meaning:'dotted cliff',freq:'15'}),
(R54:Radical{id:'54',char:'廴',altchar1:'',altchar2:'',strokes:'3',meaning:'long stride',freq:'9'}),
(R55:Radical{id:'55',char:'廾',altchar1:'',altchar2:'',strokes:'3',meaning:'two hands',freq:'50'}),
(R56:Radical{id:'56',char:'弋',altchar1:'',altchar2:'',strokes:'3',meaning:'shoot',freq:'15'}),
(R57:Radical{id:'57',char:'弓',altchar1:'',altchar2:'',strokes:'3',meaning:'bow',freq:'165'}),
(R58:Radical{id:'58',char:'彐',altchar1:'彑',altchar2:'',strokes:'3',meaning:'snout',freq:'25'}),
(R59:Radical{id:'59',char:'彡',altchar1:'',altchar2:'',strokes:'3',meaning:'bristle',freq:'62'}),
(R60:Radical{id:'60',char:'彳',altchar1:'',altchar2:'',strokes:'3',meaning:'step',freq:'215'}),
(R61:Radical{id:'61',char:'心',altchar1:'忄',altchar2:'⺗',strokes:'4',meaning:'heart',freq:'1115'}),
(R62:Radical{id:'62',char:'戈',altchar1:'',altchar2:'',strokes:'4',meaning:'halberd',freq:'116'}),
(R63:Radical{id:'63',char:'戶',altchar1:'户',altchar2:'戸',strokes:'4',meaning:'door',freq:'44'}),
(R64:Radical{id:'64',char:'手',altchar1:'扌',altchar2:'龵',strokes:'4',meaning:'hand',freq:'1'}),
(R65:Radical{id:'65',char:'支',altchar1:'',altchar2:'',strokes:'4',meaning:'branch',freq:'26'}),
(R66:Radical{id:'66',char:'攴',altchar1:'攵',altchar2:'',strokes:'4',meaning:'rap',freq:'296'}),
(R67:Radical{id:'67',char:'文',altchar1:'',altchar2:'',strokes:'4',meaning:'script',freq:'26'}),
(R68:Radical{id:'68',char:'斗',altchar1:'',altchar2:'',strokes:'4',meaning:'dipper',freq:'32'}),
(R69:Radical{id:'69',char:'斤',altchar1:'',altchar2:'',strokes:'4',meaning:'axe',freq:'55'}),
(R70:Radical{id:'70',char:'方',altchar1:'',altchar2:'',strokes:'4',meaning:'square',freq:'92'}),
(R71:Radical{id:'71',char:'无',altchar1:'旡',altchar2:'',strokes:'4',meaning:'not',freq:'12'}),
(R72:Radical{id:'72',char:'日',altchar1:'',altchar2:'',strokes:'4',meaning:'sun',freq:'453'}),
(R73:Radical{id:'73',char:'曰',altchar1:'',altchar2:'',strokes:'4',meaning:'say',freq:'37'}),
(R74:Radical{id:'74',char:'月',altchar1:'',altchar2:'',strokes:'4',meaning:'moon',freq:'69'}),
(R75:Radical{id:'75',char:'木',altchar1:'',altchar2:'',strokes:'4',meaning:'tree',freq:'1369'}),
(R76:Radical{id:'76',char:'欠',altchar1:'',altchar2:'',strokes:'4',meaning:'lack',freq:'235'}),
(R77:Radical{id:'77',char:'止',altchar1:'',altchar2:'',strokes:'4',meaning:'stop',freq:'99'}),
(R78:Radical{id:'78',char:'歹',altchar1:'歺',altchar2:'',strokes:'4',meaning:'death',freq:'231'}),
(R79:Radical{id:'79',char:'殳',altchar1:'',altchar2:'',strokes:'4',meaning:'weapon',freq:'93'}),
(R80:Radical{id:'80',char:'毋',altchar1:'母',altchar2:'⺟',strokes:'4',meaning:'do not',freq:'16'}),
(R81:Radical{id:'81',char:'比',altchar1:'',altchar2:'',strokes:'4',meaning:'compare',freq:'21'}),
(R82:Radical{id:'82',char:'毛',altchar1:'',altchar2:'',strokes:'4',meaning:'fur',freq:'211'}),
(R83:Radical{id:'83',char:'氏',altchar1:'',altchar2:'',strokes:'4',meaning:'clan',freq:'10'}),
(R84:Radical{id:'84',char:'气',altchar1:'',altchar2:'',strokes:'4',meaning:'steam',freq:'17'}),
(R85:Radical{id:'85',char:'水',altchar1:'氺',altchar2:'氵',strokes:'4',meaning:'water ',freq:'1595'}),
(R86:Radical{id:'86',char:'火',altchar1:'灬',altchar2:'',strokes:'4',meaning:'fire',freq:'639'}),
(R87:Radical{id:'87',char:'爪',altchar1:'爫',altchar2:'',strokes:'4',meaning:'claw',freq:'36'}),
(R88:Radical{id:'88',char:'父',altchar1:'',altchar2:'',strokes:'4',meaning:'father',freq:'10'}),
(R89:Radical{id:'89',char:'爻',altchar1:'',altchar2:'',strokes:'4',meaning:'double x',freq:'16'}),
(R90:Radical{id:'90',char:'爿',altchar1:'丬',altchar2:'',strokes:'4',meaning:'half tree trunk',freq:'48'}),
(R91:Radical{id:'91',char:'片',altchar1:'',altchar2:'',strokes:'4',meaning:'slice',freq:'77'}),
(R92:Radical{id:'92',char:'牙',altchar1:'',altchar2:'',strokes:'4',meaning:'fang',freq:'9'}),
(R93:Radical{id:'93',char:'牛',altchar1:'牜',altchar2:'⺧',strokes:'4',meaning:'cow',freq:'233'}),
(R94:Radical{id:'94',char:'犬',altchar1:'犭',altchar2:'',strokes:'4',meaning:'dog',freq:'444'}),
(R95:Radical{id:'95',char:'玄',altchar1:'',altchar2:'',strokes:'5',meaning:'profound',freq:'6'}),
(R96:Radical{id:'96',char:'玉',altchar1:'王',altchar2:'⺩',strokes:'5',meaning:'jade',freq:'473'}),
(R97:Radical{id:'97',char:'瓜',altchar1:'',altchar2:'',strokes:'5',meaning:'melon',freq:'55'}),
(R98:Radical{id:'98',char:'瓦',altchar1:'',altchar2:'',strokes:'5',meaning:'tile',freq:'174'}),
(R99:Radical{id:'99',char:'甘',altchar1:'',altchar2:'',strokes:'5',meaning:'sweet',freq:'22'}),
(R100:Radical{id:'100',char:'生',altchar1:'',altchar2:'',strokes:'5',meaning:'life',freq:'22'}),
(R101:Radical{id:'101',char:'用',altchar1:'',altchar2:'',strokes:'5',meaning:'use',freq:'10'}),
(R102:Radical{id:'102',char:'田',altchar1:'',altchar2:'',strokes:'5',meaning:'field',freq:'192'}),
(R103:Radical{id:'103',char:'疋',altchar1:'⺪',altchar2:'',strokes:'5',meaning:'bolt of cloth',freq:'15'}),
(R104:Radical{id:'104',char:'疒',altchar1:'',altchar2:'',strokes:'5',meaning:'sickness',freq:'526'}),
(R105:Radical{id:'105',char:'癶',altchar1:'',altchar2:'',strokes:'5',meaning:'dotted tent',freq:'15'}),
(R106:Radical{id:'106',char:'白',altchar1:'',altchar2:'',strokes:'5',meaning:'white',freq:'109'}),
(R107:Radical{id:'107',char:'皮',altchar1:'',altchar2:'',strokes:'5',meaning:'skin',freq:'94'}),
(R108:Radical{id:'108',char:'皿',altchar1:'',altchar2:'',strokes:'5',meaning:'dish',freq:'129'}),
(R109:Radical{id:'109',char:'目',altchar1:'',altchar2:'',strokes:'5',meaning:'eye',freq:'647'}),
(R110:Radical{id:'110',char:'矛',altchar1:'',altchar2:'',strokes:'5',meaning:'spear',freq:'65'}),
(R111:Radical{id:'111',char:'矢',altchar1:'',altchar2:'',strokes:'5',meaning:'arrow',freq:'64'}),
(R112:Radical{id:'112',char:'石',altchar1:'',altchar2:'',strokes:'5',meaning:'stone',freq:'499'}),
(R113:Radical{id:'113',char:'示',altchar1:'礻',altchar2:'',strokes:'5',meaning:'spirit',freq:'213'}),
(R114:Radical{id:'114',char:'禸',altchar1:'',altchar2:'',strokes:'5',meaning:'track',freq:'12'}),
(R115:Radical{id:'115',char:'禾',altchar1:'',altchar2:'',strokes:'5',meaning:'grain',freq:'431'}),
(R116:Radical{id:'116',char:'穴',altchar1:'',altchar2:'',strokes:'5',meaning:'cave',freq:'298'}),
(R117:Radical{id:'117',char:'立',altchar1:'',altchar2:'',strokes:'5',meaning:'stand',freq:'101'}),
(R118:Radical{id:'118',char:'竹',altchar1:'⺮',altchar2:'',strokes:'6',meaning:'bamboo',freq:'953'}),
(R119:Radical{id:'119',char:'米',altchar1:'',altchar2:'',strokes:'6',meaning:'rice',freq:'318'}),
(R120:Radical{id:'120',char:'糸',altchar1:'糹',altchar2:'',strokes:'6',meaning:'silk  ',freq:'823'}),
(R121:Radical{id:'121',char:'缶',altchar1:'',altchar2:'',strokes:'6',meaning:'jar',freq:'77'}),
(R122:Radical{id:'122',char:'网',altchar1:'罒',altchar2:'罓',strokes:'6',meaning:'net',freq:'163'}),
(R123:Radical{id:'123',char:'羊',altchar1:'⺶',altchar2:'⺷',strokes:'6',meaning:'sheep',freq:'156'}),
(R124:Radical{id:'124',char:'羽',altchar1:'',altchar2:'',strokes:'6',meaning:'feather',freq:'220'}),
(R125:Radical{id:'125',char:'老',altchar1:'耂',altchar2:'',strokes:'6',meaning:'old',freq:'22'}),
(R126:Radical{id:'126',char:'而',altchar1:'',altchar2:'',strokes:'6',meaning:'and',freq:'22'}),
(R127:Radical{id:'127',char:'耒',altchar1:'',altchar2:'',strokes:'6',meaning:'plow',freq:'84'}),
(R128:Radical{id:'128',char:'耳',altchar1:'',altchar2:'',strokes:'6',meaning:'ear',freq:'172'}),
(R129:Radical{id:'129',char:'聿',altchar1:'⺻',altchar2:'',strokes:'6',meaning:'brush',freq:'19'}),
(R130:Radical{id:'130',char:'肉',altchar1:'⺼',altchar2:'',strokes:'6',meaning:'meat',freq:'674'}),
(R131:Radical{id:'131',char:'臣',altchar1:'',altchar2:'',strokes:'6',meaning:'minister',freq:'16'}),
(R132:Radical{id:'132',char:'自',altchar1:'',altchar2:'',strokes:'6',meaning:'self',freq:'34'}),
(R133:Radical{id:'133',char:'至',altchar1:'',altchar2:'',strokes:'6',meaning:'arrive',freq:'24'}),
(R134:Radical{id:'134',char:'臼',altchar1:'',altchar2:'',strokes:'6',meaning:'mortar',freq:'71'}),
(R135:Radical{id:'135',char:'舌',altchar1:'',altchar2:'',strokes:'6',meaning:'tongue',freq:'31'}),
(R136:Radical{id:'136',char:'舛',altchar1:'',altchar2:'',strokes:'6',meaning:'oppose',freq:'10'}),
(R137:Radical{id:'137',char:'舟',altchar1:'',altchar2:'',strokes:'6',meaning:'boat',freq:'197'}),
(R138:Radical{id:'138',char:'艮',altchar1:'',altchar2:'',strokes:'6',meaning:'stopping',freq:'5'}),
(R139:Radical{id:'139',char:'色',altchar1:'',altchar2:'',strokes:'6',meaning:'color',freq:'21'}),
(R140:Radical{id:'140',char:'艸',altchar1:'艹',altchar2:'',strokes:'6',meaning:'grass',freq:'1902'}),
(R141:Radical{id:'141',char:'虍',altchar1:'',altchar2:'',strokes:'6',meaning:'tiger',freq:'114'}),
(R142:Radical{id:'142',char:'虫',altchar1:'',altchar2:'',strokes:'6',meaning:'insect',freq:'1067'}),
(R143:Radical{id:'143',char:'血',altchar1:'',altchar2:'',strokes:'6',meaning:'blood',freq:'60'}),
(R144:Radical{id:'144',char:'行',altchar1:'',altchar2:'',strokes:'6',meaning:'walk enclosure',freq:'53'}),
(R145:Radical{id:'145',char:'衣',altchar1:'衤',altchar2:'',strokes:'6',meaning:'clothes',freq:'607'}),
(R146:Radical{id:'146',char:'襾',altchar1:'西',altchar2:'覀',strokes:'6',meaning:'west',freq:'29'}),
(R147:Radical{id:'147',char:'見',altchar1:'',altchar2:'',strokes:'7',meaning:'see',freq:'161'}),
(R148:Radical{id:'148',char:'角',altchar1:'',altchar2:'',strokes:'7',meaning:'horn',freq:'158'}),
(R149:Radical{id:'149',char:'言',altchar1:'訁',altchar2:'',strokes:'7',meaning:'speech',freq:'861'}),
(R150:Radical{id:'150',char:'谷',altchar1:'',altchar2:'',strokes:'7',meaning:'valley',freq:'54'}),
(R151:Radical{id:'151',char:'豆',altchar1:'',altchar2:'',strokes:'7',meaning:'bean',freq:'68'}),
(R152:Radical{id:'152',char:'豕',altchar1:'',altchar2:'',strokes:'7',meaning:'pig',freq:'148'}),
(R153:Radical{id:'153',char:'豸',altchar1:'',altchar2:'',strokes:'7',meaning:'badger',freq:'140'}),
(R154:Radical{id:'154',char:'貝',altchar1:'',altchar2:'',strokes:'7',meaning:'shell',freq:'277'}),
(R155:Radical{id:'155',char:'赤',altchar1:'',altchar2:'',strokes:'7',meaning:'red',freq:'31'}),
(R156:Radical{id:'156',char:'走',altchar1:'赱',altchar2:'',strokes:'7',meaning:'run',freq:'285'}),
(R157:Radical{id:'157',char:'足',altchar1:'⻊',altchar2:'',strokes:'7',meaning:'foot',freq:'580'}),
(R158:Radical{id:'158',char:'身',altchar1:'',altchar2:'',strokes:'7',meaning:'body',freq:'97'}),
(R159:Radical{id:'159',char:'車',altchar1:'',altchar2:'',strokes:'7',meaning:'cart',freq:'361'}),
(R160:Radical{id:'160',char:'辛',altchar1:'',altchar2:'',strokes:'7',meaning:'bitter',freq:'36'}),
(R161:Radical{id:'161',char:'辰',altchar1:'',altchar2:'',strokes:'7',meaning:'morning',freq:'15'}),
(R162:Radical{id:'162',char:'辵',altchar1:'辶',altchar2:'⻌',strokes:'7',meaning:'walk',freq:'381'}),
(R163:Radical{id:'163',char:'邑',altchar1:'阝',altchar2:'',strokes:'7',meaning:'city',freq:'350'}),
(R164:Radical{id:'164',char:'酉',altchar1:'',altchar2:'',strokes:'7',meaning:'wine',freq:'290'}),
(R165:Radical{id:'165',char:'釆',altchar1:'',altchar2:'',strokes:'7',meaning:'distinguish',freq:'14'}),
(R166:Radical{id:'166',char:'里',altchar1:'',altchar2:'',strokes:'7',meaning:'village',freq:'14'}),
(R167:Radical{id:'167',char:'金',altchar1:'釒',altchar2:'',strokes:'8',meaning:'gold',freq:'806'}),
(R168:Radical{id:'168',char:'長',altchar1:'镸',altchar2:'',strokes:'8',meaning:'long',freq:'55'}),
(R169:Radical{id:'169',char:'門',altchar1:'',altchar2:'',strokes:'8',meaning:'gate',freq:'246'}),
(R170:Radical{id:'170',char:'阜',altchar1:'阝',altchar2:'',strokes:'8',meaning:'mound',freq:'348'}),
(R171:Radical{id:'171',char:'隶',altchar1:'',altchar2:'',strokes:'8',meaning:'slave',freq:'12'}),
(R172:Radical{id:'172',char:'隹',altchar1:'',altchar2:'',strokes:'8',meaning:'short-tailed bird',freq:'233'}),
(R173:Radical{id:'173',char:'雨',altchar1:'',altchar2:'',strokes:'8',meaning:'rain',freq:'298'}),
(R174:Radical{id:'174',char:'青',altchar1:'靑',altchar2:'',strokes:'8',meaning:'blue',freq:'17'}),
(R175:Radical{id:'175',char:'非',altchar1:'',altchar2:'',strokes:'8',meaning:'wrong',freq:'25'}),
(R176:Radical{id:'176',char:'面',altchar1:'靣',altchar2:'',strokes:'9',meaning:'face',freq:'66'}),
(R177:Radical{id:'177',char:'革',altchar1:'',altchar2:'',strokes:'9',meaning:'leather',freq:'305'}),
(R178:Radical{id:'178',char:'韋',altchar1:'',altchar2:'',strokes:'9',meaning:'tanned leather',freq:'100'}),
(R179:Radical{id:'179',char:'韭',altchar1:'',altchar2:'',strokes:'9',meaning:'leek',freq:'20'}),
(R180:Radical{id:'180',char:'音',altchar1:'',altchar2:'',strokes:'9',meaning:'sound',freq:'43'}),
(R181:Radical{id:'181',char:'頁',altchar1:'',altchar2:'',strokes:'9',meaning:'leaf',freq:'372'}),
(R182:Radical{id:'182',char:'風',altchar1:'𠘨',altchar2:'',strokes:'9',meaning:'wind',freq:'182'}),
(R183:Radical{id:'183',char:'飛',altchar1:'',altchar2:'',strokes:'9',meaning:'fly',freq:'92'}),
(R184:Radical{id:'184',char:'食',altchar1:'飠',altchar2:'',strokes:'9',meaning:'eat',freq:'403'}),
(R185:Radical{id:'185',char:'首',altchar1:'',altchar2:'',strokes:'9',meaning:'head',freq:'20'}),
(R186:Radical{id:'186',char:'香',altchar1:'',altchar2:'',strokes:'9',meaning:'fragrant',freq:'37'}),
(R187:Radical{id:'187',char:'馬',altchar1:'',altchar2:'',strokes:'10',meaning:'horse',freq:'472'}),
(R188:Radical{id:'188',char:'骨',altchar1:'',altchar2:'',strokes:'10',meaning:'bone',freq:'185'}),
(R189:Radical{id:'189',char:'高',altchar1:'髙',altchar2:'',strokes:'10',meaning:'tall',freq:'34'}),
(R190:Radical{id:'190',char:'髟',altchar1:'',altchar2:'',strokes:'10',meaning:'hair',freq:'243'}),
(R191:Radical{id:'191',char:'鬥',altchar1:'',altchar2:'',strokes:'10',meaning:'fight',freq:'23'}),
(R192:Radical{id:'192',char:'鬯',altchar1:'',altchar2:'',strokes:'10',meaning:'sacrificial wine',freq:'8'}),
(R193:Radical{id:'193',char:'鬲',altchar1:'',altchar2:'',strokes:'10',meaning:'cauldron',freq:'73'}),
(R194:Radical{id:'194',char:'鬼',altchar1:'',altchar2:'',strokes:'10',meaning:'ghost',freq:'141'}),
(R195:Radical{id:'195',char:'魚',altchar1:'',altchar2:'',strokes:'11',meaning:'fish',freq:'571'}),
(R196:Radical{id:'196',char:'鳥',altchar1:'',altchar2:'',strokes:'11',meaning:'bird',freq:'750'}),
(R197:Radical{id:'197',char:'鹵',altchar1:'',altchar2:'',strokes:'11',meaning:'salt',freq:'44'}),
(R198:Radical{id:'198',char:'鹿',altchar1:'',altchar2:'',strokes:'11',meaning:'deer',freq:'104'}),
(R199:Radical{id:'199',char:'麥',altchar1:'',altchar2:'',strokes:'11',meaning:'wheat',freq:'131'}),
(R200:Radical{id:'200',char:'麻',altchar1:'',altchar2:'',strokes:'11',meaning:'hemp',freq:'34'}),
(R201:Radical{id:'201',char:'黃',altchar1:'',altchar2:'',strokes:'12',meaning:'yellow',freq:'42'}),
(R202:Radical{id:'202',char:'黍',altchar1:'',altchar2:'',strokes:'12',meaning:'millet',freq:'46'}),
(R203:Radical{id:'203',char:'黑',altchar1:'',altchar2:'',strokes:'12',meaning:'black',freq:'172'}),
(R204:Radical{id:'204',char:'黹',altchar1:'',altchar2:'',strokes:'12',meaning:'embroidery',freq:'8'}),
(R205:Radical{id:'205',char:'黽',altchar1:'',altchar2:'',strokes:'13',meaning:'frog',freq:'40'}),
(R206:Radical{id:'206',char:'鼎',altchar1:'',altchar2:'',strokes:'13',meaning:'tripod',freq:'14'}),
(R207:Radical{id:'207',char:'鼓',altchar1:'',altchar2:'',strokes:'13',meaning:'drum',freq:'46'}),
(R208:Radical{id:'208',char:'鼠',altchar1:'',altchar2:'',strokes:'13',meaning:'rat',freq:'92'}),
(R209:Radical{id:'209',char:'鼻',altchar1:'',altchar2:'',strokes:'14',meaning:'nose',freq:'49'}),
(R210:Radical{id:'210',char:'齊',altchar1:'',altchar2:'',strokes:'14',meaning:'even',freq:'18'}),
(R211:Radical{id:'211',char:'齒',altchar1:'',altchar2:'',strokes:'15',meaning:'tooth',freq:'162'}),
(R212:Radical{id:'212',char:'龍',altchar1:'',altchar2:'',strokes:'16',meaning:'dragon',freq:'14'}),
(R213:Radical{id:'213',char:'龜',altchar1:'',altchar2:'',strokes:'16',meaning:'turtle',freq:'24'}),
(R214:Radical{id:'214',char:'龠',altchar1:'',altchar2:'',strokes:'17',meaning:'flute',freq:'19'})
```