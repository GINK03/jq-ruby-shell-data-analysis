# 


# snippet

## MariaDB -> json

## csv to inline json
ここの行で分割されたjsonにします
```console
$ cat ${TARGET.csv} | csv2json
```

## csvのzip codeのみを抜き出す
```console
$ cat ConsumerComplaints.csv | head -n 1000 | csvtojson | jq 'map(.["Zip Code"])'
```

## stringにパースされてしまった値をnumberに変換する
```console
$ cat ConsumerComplaints.csv | head -n 1000 | csvtojson | jq 'map( .["Zip Code"] ) ' | jq ' .[] | tonumber'
```

## Ruby製のCSV to JSON変換機
```console
$ cat vehicles.csv | ruby csv2json.rb 
```
これは、行志向のJSONなので、このままjqで処理するには色々と制限が多いのと、型推定ができていない(CSVは型情報がないのじゃ。。。)

## 型がないJSONを適切な型にキャストする
```console
$ cat ${SOME_ROW_JSONS} | ruby  type_infer.rb
```

## リストにラップアップしてjqで処理できる様にする
```console
$ cat ${ANY_ROW_JSONS} | ruby to_list.rb
```

### 例
燃料代を全てreduce関数で足し合わせてたたみこむ
```cosnole
$ cat vehicles.csv | ruby csv2json.rb  | ruby type_infer.rb | ruby to_list.rb | jq 'reduce .[].fuelCost08 as $fc (0; . + $fc)'
```
