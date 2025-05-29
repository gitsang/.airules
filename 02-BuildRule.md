# Build Usage

1. 请优先使用 Makefile 或构建脚本进行构建
2. 当你需要使用命令直接构建时
   - 请先使用 `find . -type f -name ".gitignore" -exec cat {} \;` 获取 git 忽略列表
   - 请不要将二进制文件构建到 gitignore 忽略的范围之外
   - 如果你不知道应该将二进制构建到哪里，请构建到 `/tmp` 目录
