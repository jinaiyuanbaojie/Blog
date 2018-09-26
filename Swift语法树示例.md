# SourceKitten获取语法树
> 基于SourceKit的封装
```shell
input: sourcekitten structure --text "struct A { func b() {} }"

output:
{
  "key.diagnostic_stage" : "source.diagnostic.stage.swift.parse",
  "key.length" : 24,
  "key.offset" : 0,
  "key.substructure" : [
    {
      "key.accessibility" : "source.lang.swift.accessibility.internal",
      "key.bodylength" : 13,
      "key.bodyoffset" : 10,
      "key.kind" : "source.lang.swift.decl.struct",
      "key.length" : 24,
      "key.name" : "A",
      "key.namelength" : 1,
      "key.nameoffset" : 7,
      "key.offset" : 0,
      "key.substructure" : [
        {
          "key.accessibility" : "source.lang.swift.accessibility.internal",
          "key.bodylength" : 0,
          "key.bodyoffset" : 21,
          "key.kind" : "source.lang.swift.decl.function.method.instance",
          "key.length" : 11,
          "key.name" : "b()",
          "key.namelength" : 3,
          "key.nameoffset" : 16,
          "key.offset" : 11
        }
      ]
    }
  ]
}
```
