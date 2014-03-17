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
(R1:Radical {index:'1',character:'一',altchar1:'',altchar2:'',strokes:'1',meaning:'one',frequency:'42'}),
(R2:Radical {index:'2',character:'丨',altchar1:'',altchar2:'',strokes:'1',meaning:'line',frequency:'21'}),
(R3:Radical {index:'3',character:'丶',altchar1:'',altchar2:'',strokes:'1',meaning:'dot',frequency:'10'}),
(R4:Radical {index:'4',character:'丿',altchar1:'',altchar2:'',strokes:'1',meaning:'slash',frequency:'33'}),
(R5:Radical {index:'5',character:'乙',altchar1:'⺄',altchar2:'乚',strokes:'1',meaning:'second',frequency:'42'}),
(R6:Radical {index:'6',character:'亅',altchar1:'',altchar2:'',strokes:'1',meaning:'hook',frequency:'19'}),
(R7:Radical {index:'7',character:'二',altchar1:'',altchar2:'',strokes:'2',meaning:'two',frequency:'29'}),
(R8:Radical {index:'8',character:'亠',altchar1:'',altchar2:'',strokes:'2',meaning:'lid',frequency:'38'}),
(R9:Radical {index:'9',character:'人',altchar1:'亻',altchar2:'𠆢',strokes:'2',meaning:'man',frequency:'794'}),
(R10:Radical {index:'10',character:'儿',altchar1:'',altchar2:'',strokes:'2',meaning:'legs',frequency:'52'}),
(R11:Radical {index:'11',character:'入',altchar1:'',altchar2:'',strokes:'2',meaning:'enter',frequency:'28'}),
(R12:Radical {index:'12',character:'八',altchar1:'',altchar2:'',strokes:'2',meaning:'eight',frequency:'44'}),
(R13:Radical {index:'13',character:'冂',altchar1:'',altchar2:'',strokes:'2',meaning:'down box',frequency:'50'}),
(R14:Radical {index:'14',character:'冖',altchar1:'',altchar2:'',strokes:'2',meaning:'cover',frequency:'30'}),
(R15:Radical {index:'15',character:'冫',altchar1:'',altchar2:'',strokes:'2',meaning:'ice',frequency:'115'}),
(R16:Radical {index:'16',character:'几',altchar1:'',altchar2:'',strokes:'2',meaning:'table',frequency:'38'}),
(R17:Radical {index:'17',character:'凵',altchar1:'',altchar2:'',strokes:'2',meaning:'open box',frequency:'23'}),
(R18:Radical {index:'18',character:'刀',altchar1:'刂',altchar2:'',strokes:'2',meaning:'knife',frequency:'377'}),
(R19:Radical {index:'19',character:'力',altchar1:'',altchar2:'',strokes:'2',meaning:'power',frequency:'163'}),
(R20:Radical {index:'20',character:'勹',altchar1:'',altchar2:'',strokes:'2',meaning:'wrap',frequency:'64'}),
(R21:Radical {index:'21',character:'匕',altchar1:'',altchar2:'',strokes:'2',meaning:'spoon',frequency:'19'}),
(R22:Radical {index:'22',character:'匚',altchar1:'',altchar2:'',strokes:'2',meaning:'right open box',frequency:'64'}),
(R23:Radical {index:'23',character:'匸',altchar1:'',altchar2:'',strokes:'2',meaning:'hiding enclosure',frequency:'17'}),
(R24:Radical {index:'24',character:'十',altchar1:'',altchar2:'',strokes:'2',meaning:'ten',frequency:'55'}),
(R25:Radical {index:'25',character:'卜',altchar1:'',altchar2:'',strokes:'2',meaning:'divination',frequency:'45'}),
(R26:Radical {index:'26',character:'卩',altchar1:'㔾',altchar2:'',strokes:'2',meaning:'seal',frequency:'40'}),
(R27:Radical {index:'27',character:'厂',altchar1:'',altchar2:'',strokes:'2',meaning:'cliff',frequency:'129'}),
(R28:Radical {index:'28',character:'厶',altchar1:'',altchar2:'',strokes:'2',meaning:'private',frequency:'40'}),
(R29:Radical {index:'29',character:'又',altchar1:'',altchar2:'',strokes:'2',meaning:'again',frequency:'91'}),
(R30:Radical {index:'30',character:'口',altchar1:'',altchar2:'',strokes:'3',meaning:'mouth',frequency:'1146'}),
(R31:Radical {index:'31',character:'囗',altchar1:'',altchar2:'',strokes:'3',meaning:'enclosure',frequency:'118'}),
(R32:Radical {index:'32',character:'土',altchar1:'',altchar2:'',strokes:'3',meaning:'earth',frequency:'580'}),
(R33:Radical {index:'33',character:'士',altchar1:'',altchar2:'',strokes:'3',meaning:'scholar',frequency:'24'}),
(R34:Radical {index:'34',character:'夂',altchar1:'',altchar2:'',strokes:'3',meaning:'go',frequency:'11'}),
(R35:Radical {index:'35',character:'夊',altchar1:'',altchar2:'',strokes:'3',meaning:'go slowly',frequency:'23'}),
(R36:Radical {index:'36',character:'夕',altchar1:'',altchar2:'',strokes:'3',meaning:'evening',frequency:'34'}),
(R37:Radical {index:'37',character:'大',altchar1:'',altchar2:'',strokes:'3',meaning:'big',frequency:'132'}),
(R38:Radical {index:'38',character:'女',altchar1:'',altchar2:'',strokes:'3',meaning:'woman',frequency:'681'}),
(R39:Radical {index:'39',character:'子',altchar1:'',altchar2:'',strokes:'3',meaning:'child',frequency:'83'}),
(R40:Radical {index:'40',character:'宀',altchar1:'',altchar2:'',strokes:'3',meaning:'roof',frequency:'246'}),
(R41:Radical {index:'41',character:'寸',altchar1:'',altchar2:'',strokes:'3',meaning:'inch',frequency:'40'}),
(R42:Radical {index:'42',character:'小',altchar1:'⺌',altchar2:'⺍',strokes:'3',meaning:'small',frequency:'41'}),
(R43:Radical {index:'43',character:'尢',altchar1:'尣',altchar2:'',strokes:'3',meaning:'lame',frequency:'66'}),
(R44:Radical {index:'44',character:'尸',altchar1:'',altchar2:'',strokes:'3',meaning:'corpse',frequency:'148'}),
(R45:Radical {index:'45',character:'屮',altchar1:'',altchar2:'',strokes:'3',meaning:'sprout',frequency:'38'}),
(R46:Radical {index:'46',character:'山',altchar1:'',altchar2:'',strokes:'3',meaning:'mountain',frequency:'636'}),
(R47:Radical {index:'47',character:'巛',altchar1:'川',altchar2:'巜',strokes:'3',meaning:'river',frequency:'26'}),
(R48:Radical {index:'48',character:'工',altchar1:'',altchar2:'',strokes:'3',meaning:'work',frequency:'17'}),
(R49:Radical {index:'49',character:'己',altchar1:'',altchar2:'',strokes:'3',meaning:'oneself',frequency:'20'}),
(R50:Radical {index:'50',character:'巾',altchar1:'',altchar2:'',strokes:'3',meaning:'turban',frequency:'295'}),
(R51:Radical {index:'51',character:'干',altchar1:'',altchar2:'',strokes:'3',meaning:'dry',frequency:'9'}),
(R52:Radical {index:'52',character:'幺',altchar1:'',altchar2:'',strokes:'3',meaning:'short thread',frequency:'50'}),
(R53:Radical {index:'53',character:'广',altchar1:'',altchar2:'',strokes:'3',meaning:'dotted cliff',frequency:'15'}),
(R54:Radical {index:'54',character:'廴',altchar1:'',altchar2:'',strokes:'3',meaning:'long stride',frequency:'9'}),
(R55:Radical {index:'55',character:'廾',altchar1:'',altchar2:'',strokes:'3',meaning:'two hands',frequency:'50'}),
(R56:Radical {index:'56',character:'弋',altchar1:'',altchar2:'',strokes:'3',meaning:'shoot',frequency:'15'}),
(R57:Radical {index:'57',character:'弓',altchar1:'',altchar2:'',strokes:'3',meaning:'bow',frequency:'165'}),
(R58:Radical {index:'58',character:'彐',altchar1:'彑',altchar2:'',strokes:'3',meaning:'snout',frequency:'25'}),
(R59:Radical {index:'59',character:'彡',altchar1:'',altchar2:'',strokes:'3',meaning:'bristle',frequency:'62'}),
(R60:Radical {index:'60',character:'彳',altchar1:'',altchar2:'',strokes:'3',meaning:'step',frequency:'215'}),
(R61:Radical {index:'61',character:'心',altchar1:'忄',altchar2:'⺗',strokes:'4',meaning:'heart',frequency:'1115'}),
(R62:Radical {index:'62',character:'戈',altchar1:'',altchar2:'',strokes:'4',meaning:'halberd',frequency:'116'}),
(R63:Radical {index:'63',character:'戶',altchar1:'户',altchar2:'戸',strokes:'4',meaning:'door',frequency:'44'}),
(R64:Radical {index:'64',character:'手',altchar1:'扌',altchar2:'龵',strokes:'4',meaning:'hand',frequency:'1'}),
(R65:Radical {index:'65',character:'支',altchar1:'',altchar2:'',strokes:'4',meaning:'branch',frequency:'26'}),
(R66:Radical {index:'66',character:'攴',altchar1:'攵',altchar2:'',strokes:'4',meaning:'rap',frequency:'296'}),
(R67:Radical {index:'67',character:'文',altchar1:'',altchar2:'',strokes:'4',meaning:'script',frequency:'26'}),
(R68:Radical {index:'68',character:'斗',altchar1:'',altchar2:'',strokes:'4',meaning:'dipper',frequency:'32'}),
(R69:Radical {index:'69',character:'斤',altchar1:'',altchar2:'',strokes:'4',meaning:'axe',frequency:'55'}),
(R70:Radical {index:'70',character:'方',altchar1:'',altchar2:'',strokes:'4',meaning:'square',frequency:'92'}),
(R71:Radical {index:'71',character:'无',altchar1:'旡',altchar2:'',strokes:'4',meaning:'not',frequency:'12'}),
(R72:Radical {index:'72',character:'日',altchar1:'',altchar2:'',strokes:'4',meaning:'sun',frequency:'453'}),
(R73:Radical {index:'73',character:'曰',altchar1:'',altchar2:'',strokes:'4',meaning:'say',frequency:'37'}),
(R74:Radical {index:'74',character:'月',altchar1:'',altchar2:'',strokes:'4',meaning:'moon',frequency:'69'}),
(R75:Radical {index:'75',character:'木',altchar1:'',altchar2:'',strokes:'4',meaning:'tree',frequency:'1369'}),
(R76:Radical {index:'76',character:'欠',altchar1:'',altchar2:'',strokes:'4',meaning:'lack',frequency:'235'}),
(R77:Radical {index:'77',character:'止',altchar1:'',altchar2:'',strokes:'4',meaning:'stop',frequency:'99'}),
(R78:Radical {index:'78',character:'歹',altchar1:'歺',altchar2:'',strokes:'4',meaning:'death',frequency:'231'}),
(R79:Radical {index:'79',character:'殳',altchar1:'',altchar2:'',strokes:'4',meaning:'weapon',frequency:'93'}),
(R80:Radical {index:'80',character:'毋',altchar1:'母',altchar2:'⺟',strokes:'4',meaning:'do not',frequency:'16'}),
(R81:Radical {index:'81',character:'比',altchar1:'',altchar2:'',strokes:'4',meaning:'compare',frequency:'21'}),
(R82:Radical {index:'82',character:'毛',altchar1:'',altchar2:'',strokes:'4',meaning:'fur',frequency:'211'}),
(R83:Radical {index:'83',character:'氏',altchar1:'',altchar2:'',strokes:'4',meaning:'clan',frequency:'10'}),
(R84:Radical {index:'84',character:'气',altchar1:'',altchar2:'',strokes:'4',meaning:'steam',frequency:'17'}),
(R85:Radical {index:'85',character:'水',altchar1:'氺',altchar2:'氵',strokes:'4',meaning:'water ',frequency:'1595'}),
(R86:Radical {index:'86',character:'火',altchar1:'灬',altchar2:'',strokes:'4',meaning:'fire',frequency:'639'}),
(R87:Radical {index:'87',character:'爪',altchar1:'爫',altchar2:'',strokes:'4',meaning:'claw',frequency:'36'}),
(R88:Radical {index:'88',character:'父',altchar1:'',altchar2:'',strokes:'4',meaning:'father',frequency:'10'}),
(R89:Radical {index:'89',character:'爻',altchar1:'',altchar2:'',strokes:'4',meaning:'double x',frequency:'16'}),
(R90:Radical {index:'90',character:'爿',altchar1:'丬',altchar2:'',strokes:'4',meaning:'half tree trunk',frequency:'48'}),
(R91:Radical {index:'91',character:'片',altchar1:'',altchar2:'',strokes:'4',meaning:'slice',frequency:'77'}),
(R92:Radical {index:'92',character:'牙',altchar1:'',altchar2:'',strokes:'4',meaning:'fang',frequency:'9'}),
(R93:Radical {index:'93',character:'牛',altchar1:'牜',altchar2:'⺧',strokes:'4',meaning:'cow',frequency:'233'}),
(R94:Radical {index:'94',character:'犬',altchar1:'犭',altchar2:'',strokes:'4',meaning:'dog',frequency:'444'}),
(R95:Radical {index:'95',character:'玄',altchar1:'',altchar2:'',strokes:'5',meaning:'profound',frequency:'6'}),
(R96:Radical {index:'96',character:'玉',altchar1:'王',altchar2:'⺩',strokes:'5',meaning:'jade',frequency:'473'}),
(R97:Radical {index:'97',character:'瓜',altchar1:'',altchar2:'',strokes:'5',meaning:'melon',frequency:'55'}),
(R98:Radical {index:'98',character:'瓦',altchar1:'',altchar2:'',strokes:'5',meaning:'tile',frequency:'174'}),
(R99:Radical {index:'99',character:'甘',altchar1:'',altchar2:'',strokes:'5',meaning:'sweet',frequency:'22'}),
(R100:Radical {index:'100',character:'生',altchar1:'',altchar2:'',strokes:'5',meaning:'life',frequency:'22'}),
(R101:Radical {index:'101',character:'用',altchar1:'',altchar2:'',strokes:'5',meaning:'use',frequency:'10'}),
(R102:Radical {index:'102',character:'田',altchar1:'',altchar2:'',strokes:'5',meaning:'field',frequency:'192'}),
(R103:Radical {index:'103',character:'疋',altchar1:'⺪',altchar2:'',strokes:'5',meaning:'bolt of cloth',frequency:'15'}),
(R104:Radical {index:'104',character:'疒',altchar1:'',altchar2:'',strokes:'5',meaning:'sickness',frequency:'526'}),
(R105:Radical {index:'105',character:'癶',altchar1:'',altchar2:'',strokes:'5',meaning:'dotted tent',frequency:'15'}),
(R106:Radical {index:'106',character:'白',altchar1:'',altchar2:'',strokes:'5',meaning:'white',frequency:'109'}),
(R107:Radical {index:'107',character:'皮',altchar1:'',altchar2:'',strokes:'5',meaning:'skin',frequency:'94'}),
(R108:Radical {index:'108',character:'皿',altchar1:'',altchar2:'',strokes:'5',meaning:'dish',frequency:'129'}),
(R109:Radical {index:'109',character:'目',altchar1:'',altchar2:'',strokes:'5',meaning:'eye',frequency:'647'}),
(R110:Radical {index:'110',character:'矛',altchar1:'',altchar2:'',strokes:'5',meaning:'spear',frequency:'65'}),
(R111:Radical {index:'111',character:'矢',altchar1:'',altchar2:'',strokes:'5',meaning:'arrow',frequency:'64'}),
(R112:Radical {index:'112',character:'石',altchar1:'',altchar2:'',strokes:'5',meaning:'stone',frequency:'499'}),
(R113:Radical {index:'113',character:'示',altchar1:'礻',altchar2:'',strokes:'5',meaning:'spirit',frequency:'213'}),
(R114:Radical {index:'114',character:'禸',altchar1:'',altchar2:'',strokes:'5',meaning:'track',frequency:'12'}),
(R115:Radical {index:'115',character:'禾',altchar1:'',altchar2:'',strokes:'5',meaning:'grain',frequency:'431'}),
(R116:Radical {index:'116',character:'穴',altchar1:'',altchar2:'',strokes:'5',meaning:'cave',frequency:'298'}),
(R117:Radical {index:'117',character:'立',altchar1:'',altchar2:'',strokes:'5',meaning:'stand',frequency:'101'}),
(R118:Radical {index:'118',character:'竹',altchar1:'⺮',altchar2:'',strokes:'6',meaning:'bamboo',frequency:'953'}),
(R119:Radical {index:'119',character:'米',altchar1:'',altchar2:'',strokes:'6',meaning:'rice',frequency:'318'}),
(R120:Radical {index:'120',character:'糸',altchar1:'糹',altchar2:'',strokes:'6',meaning:'silk  ',frequency:'823'}),
(R121:Radical {index:'121',character:'缶',altchar1:'',altchar2:'',strokes:'6',meaning:'jar',frequency:'77'}),
(R122:Radical {index:'122',character:'网',altchar1:'罒',altchar2:'罓',strokes:'6',meaning:'net',frequency:'163'}),
(R123:Radical {index:'123',character:'羊',altchar1:'⺶',altchar2:'⺷',strokes:'6',meaning:'sheep',frequency:'156'}),
(R124:Radical {index:'124',character:'羽',altchar1:'',altchar2:'',strokes:'6',meaning:'feather',frequency:'220'}),
(R125:Radical {index:'125',character:'老',altchar1:'耂',altchar2:'',strokes:'6',meaning:'old',frequency:'22'}),
(R126:Radical {index:'126',character:'而',altchar1:'',altchar2:'',strokes:'6',meaning:'and',frequency:'22'}),
(R127:Radical {index:'127',character:'耒',altchar1:'',altchar2:'',strokes:'6',meaning:'plow',frequency:'84'}),
(R128:Radical {index:'128',character:'耳',altchar1:'',altchar2:'',strokes:'6',meaning:'ear',frequency:'172'}),
(R129:Radical {index:'129',character:'聿',altchar1:'⺻',altchar2:'',strokes:'6',meaning:'brush',frequency:'19'}),
(R130:Radical {index:'130',character:'肉',altchar1:'⺼',altchar2:'',strokes:'6',meaning:'meat',frequency:'674'}),
(R131:Radical {index:'131',character:'臣',altchar1:'',altchar2:'',strokes:'6',meaning:'minister',frequency:'16'}),
(R132:Radical {index:'132',character:'自',altchar1:'',altchar2:'',strokes:'6',meaning:'self',frequency:'34'}),
(R133:Radical {index:'133',character:'至',altchar1:'',altchar2:'',strokes:'6',meaning:'arrive',frequency:'24'}),
(R134:Radical {index:'134',character:'臼',altchar1:'',altchar2:'',strokes:'6',meaning:'mortar',frequency:'71'}),
(R135:Radical {index:'135',character:'舌',altchar1:'',altchar2:'',strokes:'6',meaning:'tongue',frequency:'31'}),
(R136:Radical {index:'136',character:'舛',altchar1:'',altchar2:'',strokes:'6',meaning:'oppose',frequency:'10'}),
(R137:Radical {index:'137',character:'舟',altchar1:'',altchar2:'',strokes:'6',meaning:'boat',frequency:'197'}),
(R138:Radical {index:'138',character:'艮',altchar1:'',altchar2:'',strokes:'6',meaning:'stopping',frequency:'5'}),
(R139:Radical {index:'139',character:'色',altchar1:'',altchar2:'',strokes:'6',meaning:'color',frequency:'21'}),
(R140:Radical {index:'140',character:'艸',altchar1:'艹',altchar2:'',strokes:'6',meaning:'grass',frequency:'1902'}),
(R141:Radical {index:'141',character:'虍',altchar1:'',altchar2:'',strokes:'6',meaning:'tiger',frequency:'114'}),
(R142:Radical {index:'142',character:'虫',altchar1:'',altchar2:'',strokes:'6',meaning:'insect',frequency:'1067'}),
(R143:Radical {index:'143',character:'血',altchar1:'',altchar2:'',strokes:'6',meaning:'blood',frequency:'60'}),
(R144:Radical {index:'144',character:'行',altchar1:'',altchar2:'',strokes:'6',meaning:'walk enclosure',frequency:'53'}),
(R145:Radical {index:'145',character:'衣',altchar1:'衤',altchar2:'',strokes:'6',meaning:'clothes',frequency:'607'}),
(R146:Radical {index:'146',character:'襾',altchar1:'西',altchar2:'覀',strokes:'6',meaning:'west',frequency:'29'}),
(R147:Radical {index:'147',character:'見',altchar1:'',altchar2:'',strokes:'7',meaning:'see',frequency:'161'}),
(R148:Radical {index:'148',character:'角',altchar1:'',altchar2:'',strokes:'7',meaning:'horn',frequency:'158'}),
(R149:Radical {index:'149',character:'言',altchar1:'訁',altchar2:'',strokes:'7',meaning:'speech',frequency:'861'}),
(R150:Radical {index:'150',character:'谷',altchar1:'',altchar2:'',strokes:'7',meaning:'valley',frequency:'54'}),
(R151:Radical {index:'151',character:'豆',altchar1:'',altchar2:'',strokes:'7',meaning:'bean',frequency:'68'}),
(R152:Radical {index:'152',character:'豕',altchar1:'',altchar2:'',strokes:'7',meaning:'pig',frequency:'148'}),
(R153:Radical {index:'153',character:'豸',altchar1:'',altchar2:'',strokes:'7',meaning:'badger',frequency:'140'}),
(R154:Radical {index:'154',character:'貝',altchar1:'',altchar2:'',strokes:'7',meaning:'shell',frequency:'277'}),
(R155:Radical {index:'155',character:'赤',altchar1:'',altchar2:'',strokes:'7',meaning:'red',frequency:'31'}),
(R156:Radical {index:'156',character:'走',altchar1:'赱',altchar2:'',strokes:'7',meaning:'run',frequency:'285'}),
(R157:Radical {index:'157',character:'足',altchar1:'⻊',altchar2:'',strokes:'7',meaning:'foot',frequency:'580'}),
(R158:Radical {index:'158',character:'身',altchar1:'',altchar2:'',strokes:'7',meaning:'body',frequency:'97'}),
(R159:Radical {index:'159',character:'車',altchar1:'',altchar2:'',strokes:'7',meaning:'cart',frequency:'361'}),
(R160:Radical {index:'160',character:'辛',altchar1:'',altchar2:'',strokes:'7',meaning:'bitter',frequency:'36'}),
(R161:Radical {index:'161',character:'辰',altchar1:'',altchar2:'',strokes:'7',meaning:'morning',frequency:'15'}),
(R162:Radical {index:'162',character:'辵',altchar1:'辶',altchar2:'⻌',strokes:'7',meaning:'walk',frequency:'381'}),
(R163:Radical {index:'163',character:'邑',altchar1:'阝',altchar2:'',strokes:'7',meaning:'city',frequency:'350'}),
(R164:Radical {index:'164',character:'酉',altchar1:'',altchar2:'',strokes:'7',meaning:'wine',frequency:'290'}),
(R165:Radical {index:'165',character:'釆',altchar1:'',altchar2:'',strokes:'7',meaning:'distinguish',frequency:'14'}),
(R166:Radical {index:'166',character:'里',altchar1:'',altchar2:'',strokes:'7',meaning:'village',frequency:'14'}),
(R167:Radical {index:'167',character:'金',altchar1:'釒',altchar2:'',strokes:'8',meaning:'gold',frequency:'806'}),
(R168:Radical {index:'168',character:'長',altchar1:'镸',altchar2:'',strokes:'8',meaning:'long',frequency:'55'}),
(R169:Radical {index:'169',character:'門',altchar1:'',altchar2:'',strokes:'8',meaning:'gate',frequency:'246'}),
(R170:Radical {index:'170',character:'阜',altchar1:'阝',altchar2:'',strokes:'8',meaning:'mound',frequency:'348'}),
(R171:Radical {index:'171',character:'隶',altchar1:'',altchar2:'',strokes:'8',meaning:'slave',frequency:'12'}),
(R172:Radical {index:'172',character:'隹',altchar1:'',altchar2:'',strokes:'8',meaning:'short-tailed bird',frequency:'233'}),
(R173:Radical {index:'173',character:'雨',altchar1:'',altchar2:'',strokes:'8',meaning:'rain',frequency:'298'}),
(R174:Radical {index:'174',character:'青',altchar1:'靑',altchar2:'',strokes:'8',meaning:'blue',frequency:'17'}),
(R175:Radical {index:'175',character:'非',altchar1:'',altchar2:'',strokes:'8',meaning:'wrong',frequency:'25'}),
(R176:Radical {index:'176',character:'面',altchar1:'靣',altchar2:'',strokes:'9',meaning:'face',frequency:'66'}),
(R177:Radical {index:'177',character:'革',altchar1:'',altchar2:'',strokes:'9',meaning:'leather',frequency:'305'}),
(R178:Radical {index:'178',character:'韋',altchar1:'',altchar2:'',strokes:'9',meaning:'tanned leather',frequency:'100'}),
(R179:Radical {index:'179',character:'韭',altchar1:'',altchar2:'',strokes:'9',meaning:'leek',frequency:'20'}),
(R180:Radical {index:'180',character:'音',altchar1:'',altchar2:'',strokes:'9',meaning:'sound',frequency:'43'}),
(R181:Radical {index:'181',character:'頁',altchar1:'',altchar2:'',strokes:'9',meaning:'leaf',frequency:'372'}),
(R182:Radical {index:'182',character:'風',altchar1:'𠘨',altchar2:'',strokes:'9',meaning:'wind',frequency:'182'}),
(R183:Radical {index:'183',character:'飛',altchar1:'',altchar2:'',strokes:'9',meaning:'fly',frequency:'92'}),
(R184:Radical {index:'184',character:'食',altchar1:'飠',altchar2:'',strokes:'9',meaning:'eat',frequency:'403'}),
(R185:Radical {index:'185',character:'首',altchar1:'',altchar2:'',strokes:'9',meaning:'head',frequency:'20'}),
(R186:Radical {index:'186',character:'香',altchar1:'',altchar2:'',strokes:'9',meaning:'fragrant',frequency:'37'}),
(R187:Radical {index:'187',character:'馬',altchar1:'',altchar2:'',strokes:'10',meaning:'horse',frequency:'472'}),
(R188:Radical {index:'188',character:'骨',altchar1:'',altchar2:'',strokes:'10',meaning:'bone',frequency:'185'}),
(R189:Radical {index:'189',character:'高',altchar1:'髙',altchar2:'',strokes:'10',meaning:'tall',frequency:'34'}),
(R190:Radical {index:'190',character:'髟',altchar1:'',altchar2:'',strokes:'10',meaning:'hair',frequency:'243'}),
(R191:Radical {index:'191',character:'鬥',altchar1:'',altchar2:'',strokes:'10',meaning:'fight',frequency:'23'}),
(R192:Radical {index:'192',character:'鬯',altchar1:'',altchar2:'',strokes:'10',meaning:'sacrificial wine',frequency:'8'}),
(R193:Radical {index:'193',character:'鬲',altchar1:'',altchar2:'',strokes:'10',meaning:'cauldron',frequency:'73'}),
(R194:Radical {index:'194',character:'鬼',altchar1:'',altchar2:'',strokes:'10',meaning:'ghost',frequency:'141'}),
(R195:Radical {index:'195',character:'魚',altchar1:'',altchar2:'',strokes:'11',meaning:'fish',frequency:'571'}),
(R196:Radical {index:'196',character:'鳥',altchar1:'',altchar2:'',strokes:'11',meaning:'bird',frequency:'750'}),
(R197:Radical {index:'197',character:'鹵',altchar1:'',altchar2:'',strokes:'11',meaning:'salt',frequency:'44'}),
(R198:Radical {index:'198',character:'鹿',altchar1:'',altchar2:'',strokes:'11',meaning:'deer',frequency:'104'}),
(R199:Radical {index:'199',character:'麥',altchar1:'',altchar2:'',strokes:'11',meaning:'wheat',frequency:'131'}),
(R200:Radical {index:'200',character:'麻',altchar1:'',altchar2:'',strokes:'11',meaning:'hemp',frequency:'34'}),
(R201:Radical {index:'201',character:'黃',altchar1:'',altchar2:'',strokes:'12',meaning:'yellow',frequency:'42'}),
(R202:Radical {index:'202',character:'黍',altchar1:'',altchar2:'',strokes:'12',meaning:'millet',frequency:'46'}),
(R203:Radical {index:'203',character:'黑',altchar1:'',altchar2:'',strokes:'12',meaning:'black',frequency:'172'}),
(R204:Radical {index:'204',character:'黹',altchar1:'',altchar2:'',strokes:'12',meaning:'embroidery',frequency:'8'}),
(R205:Radical {index:'205',character:'黽',altchar1:'',altchar2:'',strokes:'13',meaning:'frog',frequency:'40'}),
(R206:Radical {index:'206',character:'鼎',altchar1:'',altchar2:'',strokes:'13',meaning:'tripod',frequency:'14'}),
(R207:Radical {index:'207',character:'鼓',altchar1:'',altchar2:'',strokes:'13',meaning:'drum',frequency:'46'}),
(R208:Radical {index:'208',character:'鼠',altchar1:'',altchar2:'',strokes:'13',meaning:'rat',frequency:'92'}),
(R209:Radical {index:'209',character:'鼻',altchar1:'',altchar2:'',strokes:'14',meaning:'nose',frequency:'49'}),
(R210:Radical {index:'210',character:'齊',altchar1:'',altchar2:'',strokes:'14',meaning:'even',frequency:'18'}),
(R211:Radical {index:'211',character:'齒',altchar1:'',altchar2:'',strokes:'15',meaning:'tooth',frequency:'162'}),
(R212:Radical {index:'212',character:'龍',altchar1:'',altchar2:'',strokes:'16',meaning:'dragon',frequency:'14'}),
(R213:Radical {index:'213',character:'龜',altchar1:'',altchar2:'',strokes:'16',meaning:'turtle',frequency:'24'}),
(R214:Radical {index:'214',character:'龠',altchar1:'',altchar2:'',strokes:'17',meaning:'flute',frequency:'19'})
```