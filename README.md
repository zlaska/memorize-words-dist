# memorize-words-dist

Memorize Words（爬山背单词）的**公开分发仓库**：放词书包和 APP 安装包，
供 App 的备用下载源和外部直接下载使用。

## 目录

| 目录 | 内容 |
| --- | --- |
| `decks/` | 词书本体（`.sqlite`）和媒体包（`_media.zip`，超过 20MB 时按 `.partNN` 切字节分片） |
| `releases/` | APP 安装包清单（`manifest.json`）；APK 实体挂在 Releases 资产下 |

### decks/

和主站 `https://memory.leapcat.org/decks/` **保持同名同内容**，这样 App 端
主站下载失败时，只要把文件名换到本仓库的 `decks/` 就能继续下：

```
https://raw.githubusercontent.com/zlaska/memorize-words-dist/main/decks/<文件名>
```

* 一本词书只有唯一一份媒体包；超过 20MB 时切的是**同一个 zip 的字节流**
  （`xxx_media.zip.part01`、`.part02` …），客户端必须按序号拼回整包再解压。
* `decks/SHA256SUMS` 是这一批文件的校验和，用来和主站对账。

#### 备用下载源的顺序（实测，2026-09-20）

国内直连 GitHub 很慢，所以同一个文件有三个等价地址，App 端按顺序回退：

| 优先级 | 地址前缀 | 实测速度 |
| --- | --- | --- |
| 1 | `https://memory.leapcat.org/decks/`（主站 Cloudflare） | 最快 |
| 2 | `https://cdn.jsdelivr.net/gh/zlaska/memorize-words-dist@main/decks/` | ~565 KB/s |
| 3 | `https://raw.githubusercontent.com/zlaska/memorize-words-dist/main/decks/` | ~36 KB/s |

jsDelivr 走的是 GitHub 文件 CDN，**单文件上限 20MB**：超过 20MB 的
`_media.zip` 已经按 20MB 分片，每个分片都在限内，所以三处都能取到。
改内容后 jsDelivr 有一段时间的缓存，必要时到
`https://www.jsdelivr.com/tools/purge` 手动刷新。

### releases/

APK 约 300MB，超过 GitHub 单个文件 100MB 的仓库上限，所以实体以
GitHub Release 资产的形式挂在 tag `app-latest` 下：

```
https://github.com/zlaska/memorize-words-dist/releases/download/app-latest/<固定文件名>.apk

> 换版本时**替换** `app-latest` 这个 tag 下的资产，tag 名和文件名都保持不变，
> App 端写死的地址就不用改。
```

`releases/manifest.json` 记录版本、文件名、字节数、sha256 和下载地址。

## 同步方式

本仓库内容由主仓库的流水线生成，不要手工改：

```bash
cd memorize_words/pipeline/scripts
python3 publish_dist_repo.py --checkout /path/to/memorize-words-dist --commit --push
```
