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

### releases/

APK 约 300MB，超过 GitHub 单个文件 100MB 的仓库上限，所以实体以
GitHub Release 资产的形式挂在 tag `app-latest` 下：

```
https://github.com/zlaska/memorize-words-dist/releases/download/app-latest/<文件名>.apk
```

`releases/manifest.json` 记录版本、文件名、字节数、sha256 和下载地址。

## 同步方式

本仓库内容由主仓库的流水线生成，不要手工改：

```bash
cd memorize_words/pipeline/scripts
python3 publish_dist_repo.py --checkout /path/to/memorize-words-dist --commit --push
```
