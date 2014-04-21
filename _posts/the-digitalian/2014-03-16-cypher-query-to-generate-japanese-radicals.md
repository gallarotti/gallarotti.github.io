---
layout: post
categories: 
- digitalian
- neo4j
comments: true
title: Cypher Query to Generate all Japanese Radicals
image:
  feature: fountain.jpg
  credit: Francesco Gallarotti
  creditlink: http://www.gallarotti.net
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
	print "(B$record[0]:Bushu{";
	print "id:'$record[0]',";
	print "char:'$record[1]',";
	print "altchar1:'$record[2]',";
	print "altchar2:'$record[3]',";
	print "strokes:'$record[4]',";
	print "freq:'$record[6]'";
	print "mnemonic-en:'$record[5]',";
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
(B1:Bushu{id:'1',char:'一',altchar1:'',altchar2:'',strokes:'1',freq:'42'mnemonic-en:'one',}),
(B2:Bushu{id:'2',char:'丨',altchar1:'',altchar2:'',strokes:'1',freq:'21'mnemonic-en:'line',}),
(B3:Bushu{id:'3',char:'丶',altchar1:'',altchar2:'',strokes:'1',freq:'10'mnemonic-en:'dot',}),
(B4:Bushu{id:'4',char:'丿',altchar1:'',altchar2:'',strokes:'1',freq:'33'mnemonic-en:'slash',}),
(B5:Bushu{id:'5',char:'乙',altchar1:'⺄',altchar2:'乚',strokes:'1',freq:'42'mnemonic-en:'second',}),
(B6:Bushu{id:'6',char:'亅',altchar1:'',altchar2:'',strokes:'1',freq:'19'mnemonic-en:'hook',}),
(B7:Bushu{id:'7',char:'二',altchar1:'',altchar2:'',strokes:'2',freq:'29'mnemonic-en:'two',}),
(B8:Bushu{id:'8',char:'亠',altchar1:'',altchar2:'',strokes:'2',freq:'38'mnemonic-en:'lid',}),
(B9:Bushu{id:'9',char:'人',altchar1:'亻',altchar2:'𠆢',strokes:'2',freq:'794'mnemonic-en:'man',}),
(B10:Bushu{id:'10',char:'儿',altchar1:'',altchar2:'',strokes:'2',freq:'52'mnemonic-en:'legs',}),
(B11:Bushu{id:'11',char:'入',altchar1:'',altchar2:'',strokes:'2',freq:'28'mnemonic-en:'enter',}),
(B12:Bushu{id:'12',char:'八',altchar1:'',altchar2:'',strokes:'2',freq:'44'mnemonic-en:'eight',}),
(B13:Bushu{id:'13',char:'冂',altchar1:'',altchar2:'',strokes:'2',freq:'50'mnemonic-en:'down box',}),
(B14:Bushu{id:'14',char:'冖',altchar1:'',altchar2:'',strokes:'2',freq:'30'mnemonic-en:'cover',}),
(B15:Bushu{id:'15',char:'冫',altchar1:'',altchar2:'',strokes:'2',freq:'115'mnemonic-en:'ice',}),
(B16:Bushu{id:'16',char:'几',altchar1:'',altchar2:'',strokes:'2',freq:'38'mnemonic-en:'table',}),
(B17:Bushu{id:'17',char:'凵',altchar1:'',altchar2:'',strokes:'2',freq:'23'mnemonic-en:'open box',}),
(B18:Bushu{id:'18',char:'刀',altchar1:'刂',altchar2:'',strokes:'2',freq:'377'mnemonic-en:'knife',}),
(B19:Bushu{id:'19',char:'力',altchar1:'',altchar2:'',strokes:'2',freq:'163'mnemonic-en:'power',}),
(B20:Bushu{id:'20',char:'勹',altchar1:'',altchar2:'',strokes:'2',freq:'64'mnemonic-en:'wrap',}),
(B21:Bushu{id:'21',char:'匕',altchar1:'',altchar2:'',strokes:'2',freq:'19'mnemonic-en:'spoon',}),
(B22:Bushu{id:'22',char:'匚',altchar1:'',altchar2:'',strokes:'2',freq:'64'mnemonic-en:'right open box',}),
(B23:Bushu{id:'23',char:'匸',altchar1:'',altchar2:'',strokes:'2',freq:'17'mnemonic-en:'hiding enclosure',}),
(B24:Bushu{id:'24',char:'十',altchar1:'',altchar2:'',strokes:'2',freq:'55'mnemonic-en:'ten',}),
(B25:Bushu{id:'25',char:'卜',altchar1:'',altchar2:'',strokes:'2',freq:'45'mnemonic-en:'divination',}),
(B26:Bushu{id:'26',char:'卩',altchar1:'㔾',altchar2:'',strokes:'2',freq:'40'mnemonic-en:'seal',}),
(B27:Bushu{id:'27',char:'厂',altchar1:'',altchar2:'',strokes:'2',freq:'129'mnemonic-en:'cliff',}),
(B28:Bushu{id:'28',char:'厶',altchar1:'',altchar2:'',strokes:'2',freq:'40'mnemonic-en:'private',}),
(B29:Bushu{id:'29',char:'又',altchar1:'',altchar2:'',strokes:'2',freq:'91'mnemonic-en:'again',}),
(B30:Bushu{id:'30',char:'口',altchar1:'',altchar2:'',strokes:'3',freq:'1146'mnemonic-en:'mouth',}),
(B31:Bushu{id:'31',char:'囗',altchar1:'',altchar2:'',strokes:'3',freq:'118'mnemonic-en:'enclosure',}),
(B32:Bushu{id:'32',char:'土',altchar1:'',altchar2:'',strokes:'3',freq:'580'mnemonic-en:'earth',}),
(B33:Bushu{id:'33',char:'士',altchar1:'',altchar2:'',strokes:'3',freq:'24'mnemonic-en:'scholar',}),
(B34:Bushu{id:'34',char:'夂',altchar1:'',altchar2:'',strokes:'3',freq:'11'mnemonic-en:'go',}),
(B35:Bushu{id:'35',char:'夊',altchar1:'',altchar2:'',strokes:'3',freq:'23'mnemonic-en:'go slowly',}),
(B36:Bushu{id:'36',char:'夕',altchar1:'',altchar2:'',strokes:'3',freq:'34'mnemonic-en:'evening',}),
(B37:Bushu{id:'37',char:'大',altchar1:'',altchar2:'',strokes:'3',freq:'132'mnemonic-en:'big',}),
(B38:Bushu{id:'38',char:'女',altchar1:'',altchar2:'',strokes:'3',freq:'681'mnemonic-en:'woman',}),
(B39:Bushu{id:'39',char:'子',altchar1:'',altchar2:'',strokes:'3',freq:'83'mnemonic-en:'child',}),
(B40:Bushu{id:'40',char:'宀',altchar1:'',altchar2:'',strokes:'3',freq:'246'mnemonic-en:'roof',}),
(B41:Bushu{id:'41',char:'寸',altchar1:'',altchar2:'',strokes:'3',freq:'40'mnemonic-en:'inch',}),
(B42:Bushu{id:'42',char:'小',altchar1:'⺌',altchar2:'⺍',strokes:'3',freq:'41'mnemonic-en:'small',}),
(B43:Bushu{id:'43',char:'尢',altchar1:'尣',altchar2:'',strokes:'3',freq:'66'mnemonic-en:'lame',}),
(B44:Bushu{id:'44',char:'尸',altchar1:'',altchar2:'',strokes:'3',freq:'148'mnemonic-en:'corpse',}),
(B45:Bushu{id:'45',char:'屮',altchar1:'',altchar2:'',strokes:'3',freq:'38'mnemonic-en:'sprout',}),
(B46:Bushu{id:'46',char:'山',altchar1:'',altchar2:'',strokes:'3',freq:'636'mnemonic-en:'mountain',}),
(B47:Bushu{id:'47',char:'巛',altchar1:'川',altchar2:'巜',strokes:'3',freq:'26'mnemonic-en:'river',}),
(B48:Bushu{id:'48',char:'工',altchar1:'',altchar2:'',strokes:'3',freq:'17'mnemonic-en:'work',}),
(B49:Bushu{id:'49',char:'己',altchar1:'',altchar2:'',strokes:'3',freq:'20'mnemonic-en:'oneself',}),
(B50:Bushu{id:'50',char:'巾',altchar1:'',altchar2:'',strokes:'3',freq:'295'mnemonic-en:'turban',}),
(B51:Bushu{id:'51',char:'干',altchar1:'',altchar2:'',strokes:'3',freq:'9'mnemonic-en:'dry',}),
(B52:Bushu{id:'52',char:'幺',altchar1:'',altchar2:'',strokes:'3',freq:'50'mnemonic-en:'short thread',}),
(B53:Bushu{id:'53',char:'广',altchar1:'',altchar2:'',strokes:'3',freq:'15'mnemonic-en:'dotted cliff',}),
(B54:Bushu{id:'54',char:'廴',altchar1:'',altchar2:'',strokes:'3',freq:'9'mnemonic-en:'long stride',}),
(B55:Bushu{id:'55',char:'廾',altchar1:'',altchar2:'',strokes:'3',freq:'50'mnemonic-en:'two hands',}),
(B56:Bushu{id:'56',char:'弋',altchar1:'',altchar2:'',strokes:'3',freq:'15'mnemonic-en:'shoot',}),
(B57:Bushu{id:'57',char:'弓',altchar1:'',altchar2:'',strokes:'3',freq:'165'mnemonic-en:'bow',}),
(B58:Bushu{id:'58',char:'彐',altchar1:'彑',altchar2:'',strokes:'3',freq:'25'mnemonic-en:'snout',}),
(B59:Bushu{id:'59',char:'彡',altchar1:'',altchar2:'',strokes:'3',freq:'62'mnemonic-en:'bristle',}),
(B60:Bushu{id:'60',char:'彳',altchar1:'',altchar2:'',strokes:'3',freq:'215'mnemonic-en:'step',}),
(B61:Bushu{id:'61',char:'心',altchar1:'忄',altchar2:'⺗',strokes:'4',freq:'1115'mnemonic-en:'heart',}),
(B62:Bushu{id:'62',char:'戈',altchar1:'',altchar2:'',strokes:'4',freq:'116'mnemonic-en:'halberd',}),
(B63:Bushu{id:'63',char:'戶',altchar1:'户',altchar2:'戸',strokes:'4',freq:'44'mnemonic-en:'door',}),
(B64:Bushu{id:'64',char:'手',altchar1:'扌',altchar2:'龵',strokes:'4',freq:'1'mnemonic-en:'hand',}),
(B65:Bushu{id:'65',char:'支',altchar1:'',altchar2:'',strokes:'4',freq:'26'mnemonic-en:'branch',}),
(B66:Bushu{id:'66',char:'攴',altchar1:'攵',altchar2:'',strokes:'4',freq:'296'mnemonic-en:'rap',}),
(B67:Bushu{id:'67',char:'文',altchar1:'',altchar2:'',strokes:'4',freq:'26'mnemonic-en:'script',}),
(B68:Bushu{id:'68',char:'斗',altchar1:'',altchar2:'',strokes:'4',freq:'32'mnemonic-en:'dipper',}),
(B69:Bushu{id:'69',char:'斤',altchar1:'',altchar2:'',strokes:'4',freq:'55'mnemonic-en:'axe',}),
(B70:Bushu{id:'70',char:'方',altchar1:'',altchar2:'',strokes:'4',freq:'92'mnemonic-en:'square',}),
(B71:Bushu{id:'71',char:'无',altchar1:'旡',altchar2:'',strokes:'4',freq:'12'mnemonic-en:'not',}),
(B72:Bushu{id:'72',char:'日',altchar1:'',altchar2:'',strokes:'4',freq:'453'mnemonic-en:'sun',}),
(B73:Bushu{id:'73',char:'曰',altchar1:'',altchar2:'',strokes:'4',freq:'37'mnemonic-en:'say',}),
(B74:Bushu{id:'74',char:'月',altchar1:'',altchar2:'',strokes:'4',freq:'69'mnemonic-en:'moon',}),
(B75:Bushu{id:'75',char:'木',altchar1:'',altchar2:'',strokes:'4',freq:'1369'mnemonic-en:'tree',}),
(B76:Bushu{id:'76',char:'欠',altchar1:'',altchar2:'',strokes:'4',freq:'235'mnemonic-en:'lack',}),
(B77:Bushu{id:'77',char:'止',altchar1:'',altchar2:'',strokes:'4',freq:'99'mnemonic-en:'stop',}),
(B78:Bushu{id:'78',char:'歹',altchar1:'歺',altchar2:'',strokes:'4',freq:'231'mnemonic-en:'death',}),
(B79:Bushu{id:'79',char:'殳',altchar1:'',altchar2:'',strokes:'4',freq:'93'mnemonic-en:'weapon',}),
(B80:Bushu{id:'80',char:'毋',altchar1:'母',altchar2:'⺟',strokes:'4',freq:'16'mnemonic-en:'do not',}),
(B81:Bushu{id:'81',char:'比',altchar1:'',altchar2:'',strokes:'4',freq:'21'mnemonic-en:'compare',}),
(B82:Bushu{id:'82',char:'毛',altchar1:'',altchar2:'',strokes:'4',freq:'211'mnemonic-en:'fur',}),
(B83:Bushu{id:'83',char:'氏',altchar1:'',altchar2:'',strokes:'4',freq:'10'mnemonic-en:'clan',}),
(B84:Bushu{id:'84',char:'气',altchar1:'',altchar2:'',strokes:'4',freq:'17'mnemonic-en:'steam',}),
(B85:Bushu{id:'85',char:'水',altchar1:'氺',altchar2:'氵',strokes:'4',freq:'1595'mnemonic-en:'water ',}),
(B86:Bushu{id:'86',char:'火',altchar1:'灬',altchar2:'',strokes:'4',freq:'639'mnemonic-en:'fire',}),
(B87:Bushu{id:'87',char:'爪',altchar1:'爫',altchar2:'',strokes:'4',freq:'36'mnemonic-en:'claw',}),
(B88:Bushu{id:'88',char:'父',altchar1:'',altchar2:'',strokes:'4',freq:'10'mnemonic-en:'father',}),
(B89:Bushu{id:'89',char:'爻',altchar1:'',altchar2:'',strokes:'4',freq:'16'mnemonic-en:'double x',}),
(B90:Bushu{id:'90',char:'爿',altchar1:'丬',altchar2:'',strokes:'4',freq:'48'mnemonic-en:'half tree trunk',}),
(B91:Bushu{id:'91',char:'片',altchar1:'',altchar2:'',strokes:'4',freq:'77'mnemonic-en:'slice',}),
(B92:Bushu{id:'92',char:'牙',altchar1:'',altchar2:'',strokes:'4',freq:'9'mnemonic-en:'fang',}),
(B93:Bushu{id:'93',char:'牛',altchar1:'牜',altchar2:'⺧',strokes:'4',freq:'233'mnemonic-en:'cow',}),
(B94:Bushu{id:'94',char:'犬',altchar1:'犭',altchar2:'',strokes:'4',freq:'444'mnemonic-en:'dog',}),
(B95:Bushu{id:'95',char:'玄',altchar1:'',altchar2:'',strokes:'5',freq:'6'mnemonic-en:'profound',}),
(B96:Bushu{id:'96',char:'玉',altchar1:'王',altchar2:'⺩',strokes:'5',freq:'473'mnemonic-en:'jade',}),
(B97:Bushu{id:'97',char:'瓜',altchar1:'',altchar2:'',strokes:'5',freq:'55'mnemonic-en:'melon',}),
(B98:Bushu{id:'98',char:'瓦',altchar1:'',altchar2:'',strokes:'5',freq:'174'mnemonic-en:'tile',}),
(B99:Bushu{id:'99',char:'甘',altchar1:'',altchar2:'',strokes:'5',freq:'22'mnemonic-en:'sweet',}),
(B100:Bushu{id:'100',char:'生',altchar1:'',altchar2:'',strokes:'5',freq:'22'mnemonic-en:'life',}),
(B101:Bushu{id:'101',char:'用',altchar1:'',altchar2:'',strokes:'5',freq:'10'mnemonic-en:'use',}),
(B102:Bushu{id:'102',char:'田',altchar1:'',altchar2:'',strokes:'5',freq:'192'mnemonic-en:'field',}),
(B103:Bushu{id:'103',char:'疋',altchar1:'⺪',altchar2:'',strokes:'5',freq:'15'mnemonic-en:'bolt of cloth',}),
(B104:Bushu{id:'104',char:'疒',altchar1:'',altchar2:'',strokes:'5',freq:'526'mnemonic-en:'sickness',}),
(B105:Bushu{id:'105',char:'癶',altchar1:'',altchar2:'',strokes:'5',freq:'15'mnemonic-en:'dotted tent',}),
(B106:Bushu{id:'106',char:'白',altchar1:'',altchar2:'',strokes:'5',freq:'109'mnemonic-en:'white',}),
(B107:Bushu{id:'107',char:'皮',altchar1:'',altchar2:'',strokes:'5',freq:'94'mnemonic-en:'skin',}),
(B108:Bushu{id:'108',char:'皿',altchar1:'',altchar2:'',strokes:'5',freq:'129'mnemonic-en:'dish',}),
(B109:Bushu{id:'109',char:'目',altchar1:'',altchar2:'',strokes:'5',freq:'647'mnemonic-en:'eye',}),
(B110:Bushu{id:'110',char:'矛',altchar1:'',altchar2:'',strokes:'5',freq:'65'mnemonic-en:'spear',}),
(B111:Bushu{id:'111',char:'矢',altchar1:'',altchar2:'',strokes:'5',freq:'64'mnemonic-en:'arrow',}),
(B112:Bushu{id:'112',char:'石',altchar1:'',altchar2:'',strokes:'5',freq:'499'mnemonic-en:'stone',}),
(B113:Bushu{id:'113',char:'示',altchar1:'礻',altchar2:'',strokes:'5',freq:'213'mnemonic-en:'spirit',}),
(B114:Bushu{id:'114',char:'禸',altchar1:'',altchar2:'',strokes:'5',freq:'12'mnemonic-en:'track',}),
(B115:Bushu{id:'115',char:'禾',altchar1:'',altchar2:'',strokes:'5',freq:'431'mnemonic-en:'grain',}),
(B116:Bushu{id:'116',char:'穴',altchar1:'',altchar2:'',strokes:'5',freq:'298'mnemonic-en:'cave',}),
(B117:Bushu{id:'117',char:'立',altchar1:'',altchar2:'',strokes:'5',freq:'101'mnemonic-en:'stand',}),
(B118:Bushu{id:'118',char:'竹',altchar1:'⺮',altchar2:'',strokes:'6',freq:'953'mnemonic-en:'bamboo',}),
(B119:Bushu{id:'119',char:'米',altchar1:'',altchar2:'',strokes:'6',freq:'318'mnemonic-en:'rice',}),
(B120:Bushu{id:'120',char:'糸',altchar1:'糹',altchar2:'',strokes:'6',freq:'823'mnemonic-en:'silk  ',}),
(B121:Bushu{id:'121',char:'缶',altchar1:'',altchar2:'',strokes:'6',freq:'77'mnemonic-en:'jar',}),
(B122:Bushu{id:'122',char:'网',altchar1:'罒',altchar2:'罓',strokes:'6',freq:'163'mnemonic-en:'net',}),
(B123:Bushu{id:'123',char:'羊',altchar1:'⺶',altchar2:'⺷',strokes:'6',freq:'156'mnemonic-en:'sheep',}),
(B124:Bushu{id:'124',char:'羽',altchar1:'',altchar2:'',strokes:'6',freq:'220'mnemonic-en:'feather',}),
(B125:Bushu{id:'125',char:'老',altchar1:'耂',altchar2:'',strokes:'6',freq:'22'mnemonic-en:'old',}),
(B126:Bushu{id:'126',char:'而',altchar1:'',altchar2:'',strokes:'6',freq:'22'mnemonic-en:'and',}),
(B127:Bushu{id:'127',char:'耒',altchar1:'',altchar2:'',strokes:'6',freq:'84'mnemonic-en:'plow',}),
(B128:Bushu{id:'128',char:'耳',altchar1:'',altchar2:'',strokes:'6',freq:'172'mnemonic-en:'ear',}),
(B129:Bushu{id:'129',char:'聿',altchar1:'⺻',altchar2:'',strokes:'6',freq:'19'mnemonic-en:'brush',}),
(B130:Bushu{id:'130',char:'肉',altchar1:'⺼',altchar2:'',strokes:'6',freq:'674'mnemonic-en:'meat',}),
(B131:Bushu{id:'131',char:'臣',altchar1:'',altchar2:'',strokes:'6',freq:'16'mnemonic-en:'minister',}),
(B132:Bushu{id:'132',char:'自',altchar1:'',altchar2:'',strokes:'6',freq:'34'mnemonic-en:'self',}),
(B133:Bushu{id:'133',char:'至',altchar1:'',altchar2:'',strokes:'6',freq:'24'mnemonic-en:'arrive',}),
(B134:Bushu{id:'134',char:'臼',altchar1:'',altchar2:'',strokes:'6',freq:'71'mnemonic-en:'mortar',}),
(B135:Bushu{id:'135',char:'舌',altchar1:'',altchar2:'',strokes:'6',freq:'31'mnemonic-en:'tongue',}),
(B136:Bushu{id:'136',char:'舛',altchar1:'',altchar2:'',strokes:'6',freq:'10'mnemonic-en:'oppose',}),
(B137:Bushu{id:'137',char:'舟',altchar1:'',altchar2:'',strokes:'6',freq:'197'mnemonic-en:'boat',}),
(B138:Bushu{id:'138',char:'艮',altchar1:'',altchar2:'',strokes:'6',freq:'5'mnemonic-en:'stopping',}),
(B139:Bushu{id:'139',char:'色',altchar1:'',altchar2:'',strokes:'6',freq:'21'mnemonic-en:'color',}),
(B140:Bushu{id:'140',char:'艸',altchar1:'艹',altchar2:'',strokes:'6',freq:'1902'mnemonic-en:'grass',}),
(B141:Bushu{id:'141',char:'虍',altchar1:'',altchar2:'',strokes:'6',freq:'114'mnemonic-en:'tiger',}),
(B142:Bushu{id:'142',char:'虫',altchar1:'',altchar2:'',strokes:'6',freq:'1067'mnemonic-en:'insect',}),
(B143:Bushu{id:'143',char:'血',altchar1:'',altchar2:'',strokes:'6',freq:'60'mnemonic-en:'blood',}),
(B144:Bushu{id:'144',char:'行',altchar1:'',altchar2:'',strokes:'6',freq:'53'mnemonic-en:'walk enclosure',}),
(B145:Bushu{id:'145',char:'衣',altchar1:'衤',altchar2:'',strokes:'6',freq:'607'mnemonic-en:'clothes',}),
(B146:Bushu{id:'146',char:'襾',altchar1:'西',altchar2:'覀',strokes:'6',freq:'29'mnemonic-en:'west',}),
(B147:Bushu{id:'147',char:'見',altchar1:'',altchar2:'',strokes:'7',freq:'161'mnemonic-en:'see',}),
(B148:Bushu{id:'148',char:'角',altchar1:'',altchar2:'',strokes:'7',freq:'158'mnemonic-en:'horn',}),
(B149:Bushu{id:'149',char:'言',altchar1:'訁',altchar2:'',strokes:'7',freq:'861'mnemonic-en:'speech',}),
(B150:Bushu{id:'150',char:'谷',altchar1:'',altchar2:'',strokes:'7',freq:'54'mnemonic-en:'valley',}),
(B151:Bushu{id:'151',char:'豆',altchar1:'',altchar2:'',strokes:'7',freq:'68'mnemonic-en:'bean',}),
(B152:Bushu{id:'152',char:'豕',altchar1:'',altchar2:'',strokes:'7',freq:'148'mnemonic-en:'pig',}),
(B153:Bushu{id:'153',char:'豸',altchar1:'',altchar2:'',strokes:'7',freq:'140'mnemonic-en:'badger',}),
(B154:Bushu{id:'154',char:'貝',altchar1:'',altchar2:'',strokes:'7',freq:'277'mnemonic-en:'shell',}),
(B155:Bushu{id:'155',char:'赤',altchar1:'',altchar2:'',strokes:'7',freq:'31'mnemonic-en:'red',}),
(B156:Bushu{id:'156',char:'走',altchar1:'赱',altchar2:'',strokes:'7',freq:'285'mnemonic-en:'run',}),
(B157:Bushu{id:'157',char:'足',altchar1:'⻊',altchar2:'',strokes:'7',freq:'580'mnemonic-en:'foot',}),
(B158:Bushu{id:'158',char:'身',altchar1:'',altchar2:'',strokes:'7',freq:'97'mnemonic-en:'body',}),
(B159:Bushu{id:'159',char:'車',altchar1:'',altchar2:'',strokes:'7',freq:'361'mnemonic-en:'cart',}),
(B160:Bushu{id:'160',char:'辛',altchar1:'',altchar2:'',strokes:'7',freq:'36'mnemonic-en:'bitter',}),
(B161:Bushu{id:'161',char:'辰',altchar1:'',altchar2:'',strokes:'7',freq:'15'mnemonic-en:'morning',}),
(B162:Bushu{id:'162',char:'辵',altchar1:'辶',altchar2:'⻌',strokes:'7',freq:'381'mnemonic-en:'walk',}),
(B163:Bushu{id:'163',char:'邑',altchar1:'阝',altchar2:'',strokes:'7',freq:'350'mnemonic-en:'city',}),
(B164:Bushu{id:'164',char:'酉',altchar1:'',altchar2:'',strokes:'7',freq:'290'mnemonic-en:'wine',}),
(B165:Bushu{id:'165',char:'釆',altchar1:'',altchar2:'',strokes:'7',freq:'14'mnemonic-en:'distinguish',}),
(B166:Bushu{id:'166',char:'里',altchar1:'',altchar2:'',strokes:'7',freq:'14'mnemonic-en:'village',}),
(B167:Bushu{id:'167',char:'金',altchar1:'釒',altchar2:'',strokes:'8',freq:'806'mnemonic-en:'gold',}),
(B168:Bushu{id:'168',char:'長',altchar1:'镸',altchar2:'',strokes:'8',freq:'55'mnemonic-en:'long',}),
(B169:Bushu{id:'169',char:'門',altchar1:'',altchar2:'',strokes:'8',freq:'246'mnemonic-en:'gate',}),
(B170:Bushu{id:'170',char:'阜',altchar1:'阝',altchar2:'',strokes:'8',freq:'348'mnemonic-en:'mound',}),
(B171:Bushu{id:'171',char:'隶',altchar1:'',altchar2:'',strokes:'8',freq:'12'mnemonic-en:'slave',}),
(B172:Bushu{id:'172',char:'隹',altchar1:'',altchar2:'',strokes:'8',freq:'233'mnemonic-en:'short-tailed bird',}),
(B173:Bushu{id:'173',char:'雨',altchar1:'',altchar2:'',strokes:'8',freq:'298'mnemonic-en:'rain',}),
(B174:Bushu{id:'174',char:'青',altchar1:'靑',altchar2:'',strokes:'8',freq:'17'mnemonic-en:'blue',}),
(B175:Bushu{id:'175',char:'非',altchar1:'',altchar2:'',strokes:'8',freq:'25'mnemonic-en:'wrong',}),
(B176:Bushu{id:'176',char:'面',altchar1:'靣',altchar2:'',strokes:'9',freq:'66'mnemonic-en:'face',}),
(B177:Bushu{id:'177',char:'革',altchar1:'',altchar2:'',strokes:'9',freq:'305'mnemonic-en:'leather',}),
(B178:Bushu{id:'178',char:'韋',altchar1:'',altchar2:'',strokes:'9',freq:'100'mnemonic-en:'tanned leather',}),
(B179:Bushu{id:'179',char:'韭',altchar1:'',altchar2:'',strokes:'9',freq:'20'mnemonic-en:'leek',}),
(B180:Bushu{id:'180',char:'音',altchar1:'',altchar2:'',strokes:'9',freq:'43'mnemonic-en:'sound',}),
(B181:Bushu{id:'181',char:'頁',altchar1:'',altchar2:'',strokes:'9',freq:'372'mnemonic-en:'leaf',}),
(B182:Bushu{id:'182',char:'風',altchar1:'𠘨',altchar2:'',strokes:'9',freq:'182'mnemonic-en:'wind',}),
(B183:Bushu{id:'183',char:'飛',altchar1:'',altchar2:'',strokes:'9',freq:'92'mnemonic-en:'fly',}),
(B184:Bushu{id:'184',char:'食',altchar1:'飠',altchar2:'',strokes:'9',freq:'403'mnemonic-en:'eat',}),
(B185:Bushu{id:'185',char:'首',altchar1:'',altchar2:'',strokes:'9',freq:'20'mnemonic-en:'head',}),
(B186:Bushu{id:'186',char:'香',altchar1:'',altchar2:'',strokes:'9',freq:'37'mnemonic-en:'fragrant',}),
(B187:Bushu{id:'187',char:'馬',altchar1:'',altchar2:'',strokes:'10',freq:'472'mnemonic-en:'horse',}),
(B188:Bushu{id:'188',char:'骨',altchar1:'',altchar2:'',strokes:'10',freq:'185'mnemonic-en:'bone',}),
(B189:Bushu{id:'189',char:'高',altchar1:'髙',altchar2:'',strokes:'10',freq:'34'mnemonic-en:'tall',}),
(B190:Bushu{id:'190',char:'髟',altchar1:'',altchar2:'',strokes:'10',freq:'243'mnemonic-en:'hair',}),
(B191:Bushu{id:'191',char:'鬥',altchar1:'',altchar2:'',strokes:'10',freq:'23'mnemonic-en:'fight',}),
(B192:Bushu{id:'192',char:'鬯',altchar1:'',altchar2:'',strokes:'10',freq:'8'mnemonic-en:'sacrificial wine',}),
(B193:Bushu{id:'193',char:'鬲',altchar1:'',altchar2:'',strokes:'10',freq:'73'mnemonic-en:'cauldron',}),
(B194:Bushu{id:'194',char:'鬼',altchar1:'',altchar2:'',strokes:'10',freq:'141'mnemonic-en:'ghost',}),
(B195:Bushu{id:'195',char:'魚',altchar1:'',altchar2:'',strokes:'11',freq:'571'mnemonic-en:'fish',}),
(B196:Bushu{id:'196',char:'鳥',altchar1:'',altchar2:'',strokes:'11',freq:'750'mnemonic-en:'bird',}),
(B197:Bushu{id:'197',char:'鹵',altchar1:'',altchar2:'',strokes:'11',freq:'44'mnemonic-en:'salt',}),
(B198:Bushu{id:'198',char:'鹿',altchar1:'',altchar2:'',strokes:'11',freq:'104'mnemonic-en:'deer',}),
(B199:Bushu{id:'199',char:'麥',altchar1:'',altchar2:'',strokes:'11',freq:'131'mnemonic-en:'wheat',}),
(B200:Bushu{id:'200',char:'麻',altchar1:'',altchar2:'',strokes:'11',freq:'34'mnemonic-en:'hemp',}),
(B201:Bushu{id:'201',char:'黃',altchar1:'',altchar2:'',strokes:'12',freq:'42'mnemonic-en:'yellow',}),
(B202:Bushu{id:'202',char:'黍',altchar1:'',altchar2:'',strokes:'12',freq:'46'mnemonic-en:'millet',}),
(B203:Bushu{id:'203',char:'黑',altchar1:'',altchar2:'',strokes:'12',freq:'172'mnemonic-en:'black',}),
(B204:Bushu{id:'204',char:'黹',altchar1:'',altchar2:'',strokes:'12',freq:'8'mnemonic-en:'embroidery',}),
(B205:Bushu{id:'205',char:'黽',altchar1:'',altchar2:'',strokes:'13',freq:'40'mnemonic-en:'frog',}),
(B206:Bushu{id:'206',char:'鼎',altchar1:'',altchar2:'',strokes:'13',freq:'14'mnemonic-en:'tripod',}),
(B207:Bushu{id:'207',char:'鼓',altchar1:'',altchar2:'',strokes:'13',freq:'46'mnemonic-en:'drum',}),
(B208:Bushu{id:'208',char:'鼠',altchar1:'',altchar2:'',strokes:'13',freq:'92'mnemonic-en:'rat',}),
(B209:Bushu{id:'209',char:'鼻',altchar1:'',altchar2:'',strokes:'14',freq:'49'mnemonic-en:'nose',}),
(B210:Bushu{id:'210',char:'齊',altchar1:'',altchar2:'',strokes:'14',freq:'18'mnemonic-en:'even',}),
(B211:Bushu{id:'211',char:'齒',altchar1:'',altchar2:'',strokes:'15',freq:'162'mnemonic-en:'tooth',}),
(B212:Bushu{id:'212',char:'龍',altchar1:'',altchar2:'',strokes:'16',freq:'14'mnemonic-en:'dragon',}),
(B213:Bushu{id:'213',char:'龜',altchar1:'',altchar2:'',strokes:'16',freq:'24'mnemonic-en:'turtle',}),
(B214:Bushu{id:'214',char:'龠',altchar1:'',altchar2:'',strokes:'17',freq:'19'mnemonic-en:'flute',})
```