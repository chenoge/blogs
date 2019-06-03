---
title: npm依赖包版本号
date: 2018-12-05 13:05:03
tags: [npm,版本号]
---

### 一、版本号

版本号的格式：`主版本号(major).次版本号(minor).补丁版本号(patch)`

- `主版本号`： 新的架构调整，不兼容老版本
- `次版本号`： 新增功能，兼容老版本
- `补丁版本号`： 修复bug，兼容老版本

<!--more-->

<br/>



[Dependencies规则](<https://docs.npmjs.com/files/package.json>)

- `version` Must match `version` exactly
- `>version` Must be greater than `version`
- `>=version` etc
- `<version`
- `<=version`



- `version1 - version2` Same as `>=version1 <=version2`.
- `1.2.x` 1.2.0, 1.2.1, etc., but not 1.3.0
- `range1 || range2` Passes if either range1 or range2 are satisfied.



- `~version` “Approximately equivalent to version” See [semver](https://docs.npmjs.com/misc/semver)
- `^version` “Compatible with version” See [semver](https://docs.npmjs.com/misc/semver)
- `*` Matches any version
- `""` (just an empty string) Same as `*`



- `http://...` See ‘URLs as Dependencies’ below
- `git...` See ‘Git URLs as Dependencies’ below
- `user/repo` See ‘GitHub URLs’ below
- `tag` A specific version tagged and published as `tag` See `npm-dist-tag`
- `path/path/path` See [Local Paths](https://docs.npmjs.com/files/package.json#local-paths) below



```
{ "dependencies" :
  { "foo" : "1.0.0 - 2.9999.9999"
  , "bar" : ">=1.0.2 <2.1.2"
  , "baz" : ">1.0.2 <=2.3.4"
  , "boo" : "2.0.1"
  , "qux" : "<1.0.0 || >=2.3.1 <2.4.5 || >=2.5.2 <3.0.0"
  , "asd" : "http://asdf.com/asdf.tar.gz"
  , "til" : "~1.2"
  , "elf" : "~1.2.3"
  , "two" : "2.x"
  , "thr" : "3.3.x"
  , "lat" : "latest"
  , "dyl" : "file:../dyl"
  }
}
```



```
~version：大概匹配某个版本
1、如果minor版本号指定了，那么minor版本号不变，而patch版本号任意
2、如果minor和patch版本号未指定，那么minor和patch版本号任意

1.1.2  <=  ~1.1.2  <1.2.0
1.1.0  <=  ~1.1    <1.2.0
1.0.0  <=  ~1      <2.0.0
```



```
^version：兼容某个版本
1、版本号中最左边的非0数字的右侧可以任意
2、如果缺少某个版本号，则这个版本号的位置可以任意

1.1.2  <=  ^1.1.2  < 2.0.0
0.2.3  <=  ^0.2.3  < 0.3.0
0.0.0  <=  ^0.0    < 0.1.0
```