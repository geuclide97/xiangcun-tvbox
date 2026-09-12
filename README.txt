乡村电视2 点播源 — TVBox 导入说明
==============================================

上游接口: http://66.kkwk666.top:88/vod_main.php?type={0..3}&page={N}  (page 从 0 开始)
URL 加密: 自定义字母表 + 周期7 维吉尼亚 -> base64
  ALPHA = 8U243f670JGbIdW5ghijklnmDprqstuvwYyOANCoEFaHc9KLBMzPQSRT1VeXxZ
  KEY   = [7, 7, 3, 9, 4, 7, 5]

条目总数: 14298
总集数  : 180106
分类分布: {'动漫': 527, '电影': 8711, '电视剧': 3698, '综艺': 1362}

文件清单:
  config.json            TVBox 订阅配置 <- 设备里填这个文件的 URL
  xiangcun_movie.json    电影    8711 条
  xiangcun_tv.json       电视剧  3698 条
  xiangcun_variety.json  综艺    1362 条
  xiangcun_anime.json    动漫    527 条
  xiangcun_vod.json      全量    14298 条 (约 24MB, jsDelivr 不收, 本地/自建才用)

CDN 主机 (35 个):
  v13.wsyzym3u8.com                     24566
  v14.wsyzym3u8.com                     23887
  v5.wsyzym3u8.com                      15079
  v4.wsyzym3u8.com                      13987
  v6.wsyzym3u8.com                      11569
  v3.wsyzym3u8.com                       7045
  v2.wsyzym3u8.com                       6893
  svip.xgplay20.com                      6743
  v8.wsyzym3u8.com                       6278
  v11.wsyzym3u8.com                      6132
  v12.wsyzym3u8.com                      5935
  v9.wsyzym3u8.com                       5471
  svip.xgplay17.com                      5276
  svip.xgplay15.com                      2156
  svip.xgplay13.com                      1620
  svip.xgplay12.com                      1241
  svip.xgplay18.com                      1093
  v1.wsyzym3u8.com                        864
  v7.wsyzym3u8.com                        839
  svip.xgplay2.com                        828
  svip.xgplay14.com                       627
  svip.xgplay21.com                       613
  svip.xgplay7.com                        345
  play.ly166.com:65                       311
  svip.xgplay4.com                        252
  v15.wsyzym3u8.com                       176
  v4.tlkqc.com                             33
  svip.xgplay1.com                         28
  162.209.204.58                           20
  svip.xgplay16.com                         6
  v12.gggread.com                           2
  svip.xgplay19.com                         1
  v10.gggread.com                           1
  v11.gggread.com                           1
  v1.qrssuv.com                             1

已丢弃的条目 (34 条):
  上游 <url> 字段为占位 "0" —— 有片名有海报但无任何播放地址

TVBox 用法 (远程):
  1) 把 config.json + 4 个 xiangcun_*.json 传到同一个目录
  2) TVBox 设置里填 config.json 的 URL
     原生 https://raw.githubusercontent.com/<user>/<repo>/<branch>/<dir>/config.json
     jsDelivr https://cdn.jsdelivr.net/gh/<user>/<repo>@<branch>/<dir>/config.json
  3) site 里的 "./xxx.json" 会被 TVBox 自动解析成同目录地址, 不用手改

TVBox 用法 (本地):
  把 4 个分片 (或全量) 拷到设备 /TVBox/ 目录, site 的 api 用
  clan://localhost/TVBox/xxx.json

动态方案 cms_server.py (局域网, 同步+搜索):
  python cms_server.py 9978
  TVBox site: {"key":"xiangcun","name":"乡村电视2","type":1,
               "api":"http://<电脑IP>:9978/api.php/provide/vod/",
               "searchable":1,"quickSearch":1,"filterable":1}
  - 后台每 10 分钟增量同步上游新片 (进"最新上架"分类), 上游挂了不影响已加载库
  - 本地内存搜索, 14298 条约 10ms
  - 真分页 (每页 20 条)
  - 自检: http://127.0.0.1:9978/status
  适合电视盒子和电脑同一 WiFi 的场景

静态托管的固有限制:
  静态文件不理会 URL 上的 ac/t/pg/wd 参数, 永远返回整个文件。
  => 点分类 = 显示该分片全部内容 (不会按子类过滤)
  => 搜索 = 返回该分片全部内容 (不会按关键词过滤)
  => 无同步: 上传即快照, 上游新片看不到
  想要真正的分页/搜索/同步, 用 cms_server.py, 见 GITHUB_DEPLOY.md

重新生成:
  cd work && python gen_tvbox.py    # 读 all2/*.xml, 不联网
