---
layout: post
title: "Linux 图片批量压缩工具 ImageMagick"
date: 2022-02-24
tag: Linux
---

# Linux 图片批量压缩工具 ImageMagick

## 安装

环境 CentOS
安装命令：`yum install ImageMagick`

## convert压缩命令

通过正则查找当前目录下所有大于 50k 的图片，进行等比例50%的缩放；

```bash
find ./ -regex '.*\(jpg\|JPG\|png\|PNG\|jpeg\)' -size +50k -exec convert -resize 50%x50% {} {} \;
```

通过正则查找当前目录下所有大于 50k 的图片，进行像素大小控制，convert 是会自动按照最大尺寸等比例进行缩小的；

```bash
find ./ -regex '.*\(jpg\|JPG\|png\|PNG\|jpeg\)' -size +50k -exec convert -resize 500x500 {} {} \;
```

如果想降低图片的质量,可以用 convert 的 -quality 参数,质量值为 0-100 之间的数值,数字越大,质量越好,一般指定 70-80 ,基本上看不出前后的差别

```sh
convert -resize 500x500 -quality 75 xxx.jpg xxx.png 
```
同时改变质量和文件大小

```shell
find ./ -regex '.*\(png\|PNG\|jpeg\)' -size +100k -exec convert -resize 400x400 -quality 5 {} {} \;
```

------

## 通过 crontab 进行定时图片压缩

举例：对 `/www/images/` 文件夹下的所有图片每 5 分钟进行一次图片压缩处理;

-   在 `/www/images/` 下新建 convert.sh 脚本，内容：

```bash
find /www/images/ -regex '.*\(jpg\|JPG\|png\|PNG\|jpeg\)' -size +50k -exec convert -resize 50%x50% {} {} \;
```

-   通过 `crontab -e` 在文件后添加：

```ruby
*/5 * * * * /www/images/convert.sh
```

写在最后：
 当然图片压缩理应在上传时处理，不过在紧急情况下，此方法也能一解燃眉之急，最稳的办法还是建议上传的时候，进行图片压缩处理。