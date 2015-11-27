# 細菌検査オーダー/結果
更新日: 2015/11/27


## 趣旨
本事業では細菌検査を検体検査の一部であると考え、細菌検査オーダーはOML-01(OML^O33)、細菌検査結果通知はOML-11(OUL^R22)に出力することを求めている。また、塗抹検査や培養検査の結果としての菌名にJANISコードを付番し出力することを求めている。JAHIS 臨床検査データ交換規約 Ver.3.1の例示をもとに、以下に出力方法を例示する。説明に関係の無いフィールドや、検査項目等のローカルコードは省略している。また、例示中のJLAC10は一例であり、病院で採用している検査項目、測定法、材料によって異なる。

### オーダー

同一材料ごとにオーダーを出力する。

```
SPM|1|...|...|017^血液^JC10^01^血液^99aaa|...
ORC|NW|...
OBX|1|...|6A010000001770400^塗抹鏡検(一般細菌)^JC10|...
OBX|2|...|6B010000001774200^培養同定(一般細菌)^JC10|...
OBX|3|...|6C205000001776200^薬剤感受性検査-MIC^JC10|...
```

### 塗抹検査結果

結果は菌名と菌量の2行1組となる
* OBX-3(検査項目): 検査の識別にはJLAC10を利用する。
* OBX-4(検査副ID): 2行が1組であることがわかる識別子を出力する。
* OBX-5(結果値): 1行は菌名を出力し、菌の識別にはJANISコードを利用する。もう1行は菌量を出力する。

```
OBR|1|...|...|6A010000001770400^塗抹鏡検(一般細菌)^JC10|...
ORC|SC|...
OBX|1|CWE|6A010000001770414^塗抹菌名^JC10|1|1011^グラム陽性球菌^JANIS|...
OBX|2|ST|6A010000001770411^塗抹菌量^JC10|1| 3 +|...
OBX|3|CWE|6A010000001770414^塗抹菌名^JC10|2|1012^グラム陽性桿菌^JANIS|...
OBX|4|ST|6A010000001770411^塗抹菌量^JC10|2|  -|...
OBX|5|CWE|6A010000001770414^塗抹菌名^JC10|3|1011^グラム陰性球菌^JANIS|...
OBX|6|ST|6A010000001770411^塗抹菌量^JC10|3|  -|...
OBX|7|CWE|6A010000001770414^塗抹菌名^JC10|4|1012^グラム陰性桿菌^JANIS|...
OBX|8|ST|6A010000001770411^塗抹菌量^JC10|4| 2 +|...
...
```

### 培養検査結果

塗抹検査結果と同様

```
OBR|2|...|...|6B010000001774200^培養同定(一般細菌)^JC10|...
ORC|SC|...
OBX|1|CWE|6B010000001774214^培養同定菌名^JC10|1|2001^E.coli^JANIS|...
OBX|2|ST|6B010000001774211^培養同定菌量^JC10|1| 2 +|...
OBX|1|CWE|6B010000001774214^培養同定菌名^JC10|2|1301^S.aureus^JANIS|...
OBX|2|ST|6B010000001774211^培養同定菌量^JC10|2| 3 +|...
...
```

### 薬剤感受性検査結果
下記のいずれかとする

#### 薬剤をJLAC10コードで識別する場合

培養検査で検出された菌ごとに薬剤感受性検査の結果を出力する。
* OBR-26(親結果): 培養検査結果のOBX-3～5相当の情報を出力する。
* OBX-3(検査項目): 検査の識別にはJLAC10を利用する。JLAC10の識別コードで薬剤を識別する。
* OBX-5(結果値): MIC値やS/I/R等の文字を出力する。例はMIC値。

```
OBR|3|...|...|6C205000001776200^薬剤感受性検査-MIC^JC10|...OBR-4～25まで省略...|6B010000001774214&培養同定菌名&JC10^1^2001&E.coli&JANIS|...
ORC|SC|...
OBX|1|ST|6C205604701776205^PIPC(ピペラシリン)^JC10||< 2|ug/mL^μg/mL^ISO+|...|S|...
OBX|2|NM|6C205609401776205^CAZ(セフタジジム)^JC10||4|ug/mL^μg/mL^ISO+|...|S|...
...
OBR|4|...|...|6C205000001776200^薬剤感受性検査-MIC^JC10|...OBR-4～25まで省略...|6B010000001774214&培養同定菌名&JC10^2^1301&S.aureus&JANIS|...
ORC|SC|...
OBX|1|NM|6C205604701776205^PIPC(ピペラシリン)^JC10||32|ug/mL^μg/mL^ISO+|...|R|...
OBX|2|NM|6C205609401776205^CAZ(セフタジジム)^JC10||80|ug/mL^μg/mL^ISO+|...|R|...
...
```

#### 薬剤感受性結果 薬剤をJANISコードで識別する場合

JAHIS 臨床検査データ交換規約Ver.3.1 P付-11 SS-MIX2ではOBX-3にJLAC10を出力することを求めているが、『9.細菌検査(培養同定・感受性試験)の例』によると薬剤をJANISコードで識別することも可能である。
よって、塗抹検査や培養検査の結果のように、結果を2行1組で出力することで、薬剤をJANISコードで識別することも可能と考える。
* OBX-3(検査項目): 検査の識別にはJLAC10を利用する。JLAC10の識別コードは0000とする。
* OBX-5(結果値): 1行は薬剤名で、薬剤の識別にはJANISコードを利用する。もう1行はMIC値等の結果値を出力する。

```
OBR|3|...|...|6C205000001776200^薬剤感受性検査-MIC^JC10|...OBR-4～25まで省略...|6B010000001774214&培養同定菌名&JC10^1^2001&E.coli&JANIS|...
ORC|SC|...
OBX|1|CWE|6C205000001776214^薬剤名^JC10|1|1266^PIPC(ピペラシリン)^JANIS|...
OBX|2|ST|6C205000001776201^MIC値^JC10|1|< 2|ug/mL^μg/mL^ISO+|...|S|...
OBX|3|CWE|6C205000001776214^薬剤名^JC10|2|4|1661^CAZ(セフタジジム)^JANIS|...|S|...
OBX|4|NM|6C205000001776201^MIC値^JC10|2|4|ug/mL^μg/mL^ISO+|...|S|...
...
OBR|4|...|...|6C205000001776200^薬剤感受性検査-MIC^JC10|...OBR-4～25まで省略...|6B010000001774214&培養同定菌名&JC10^2^1301&S.aureus&JANIS|...
ORC|SC|...
OBX|1|CWE|6C205000001776214^薬剤名^JC10|1|1266^PIPC(ピペラシリン)^JANIS|...
OBX|2|NM|6C205000001776201^MIC値^JC10|1|32|ug/mL^μg/mL^ISO+|...|S|...
OBX|3|CWE|6C205000001776214^薬剤名^JC10|2|4|1661^CAZ(セフタジジム)^JANIS|...|S|...
OBX|4|NM|6C205000001776201^MIC値^JC10|2|80|ug/mL^μg/mL^ISO+|...|S|...