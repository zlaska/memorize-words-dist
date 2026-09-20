# releases/

这里放"以后要释放的 APP 安装包"的入口和清单。

APK 现在约 300MB，超过 GitHub 仓库单文件 100MB 的硬限制，所以实体不能直接躺在这个目录里，
只能作为 **GitHub Release 资产** 挂在固定的 tag `app-latest` 下：

```
https://github.com/zlaska/memorize-words-dist/releases/download/app-latest/MemorizeWords-v0.87.0.apk
```

* `manifest.json`：本批 APK 的文件名、字节数、sha256、下载地址，由
  `memorize_words/pipeline/scripts/publish_dist_repo.py` 生成，不要手改。
* 换版本时：在 `app-latest` 这个 tag 上**替换**资产（Tag 固定不变，App 端不需要改地址），
  然后重跑 `publish_dist_repo.py` 刷新 `manifest.json`。
* 单个 Release 资产上限 2GB，300MB 的 APK 没问题。
* 如果以后出几百 KB 的小文件（差分包、配置、索引），可以直接放这个目录。

手工上传（没有 token 时）：仓库页面 → Releases → 编辑 `app-latest` → 上传/替换 APK → Publish。
