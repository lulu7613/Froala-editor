
## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

# Froala 富文本編輯器

練習範例 [Github](https://github.com/lulu7613/Froala-editor)

![](https://i.imgur.com/oltwBNf.png)



## 參考文章

- [官方文件](https://froala.com/)
- [Vue-froala-wysiwyg Github](https://github.com/froala/vue-froala-wysiwyg)
- [几款主流好用的富文本编辑器（所见即所得常用编辑器）介绍](https://blog.csdn.net/davidhzq/article/details/100842866)
- [超好用的富文本编辑器 vue-froala-wysiwyg](https://blog.csdn.net/weixin_43222302/article/details/103068692)


---

## 使用心得

- 支援源碼編輯
- 表格功能完善
- 字體顏色跟背景顏色多了 16 進位選項，可以自定義顏色
- 上傳圖片可以使用 Froala 提供的方法 image.beforeUpload 自定義
- 相較於 Quill 更適合 H3 後台專案使用

---

## 注意事項

### 隱藏未付費提示

因 Froala 是要附費的(前端開源、後台收費)，如果未付費，編輯器會有一段文字提示，如圖: 

![](https://i.imgur.com/7rHoZf3.png)

**解決方法**: 使用 css 將這塊圖去掉

```sass=
a[href="https://froala.com/wysiwyg-editor"],
a[href="https://www.froala.com/wysiwyg-editor?k=u"] {
  padding: 0 !important;
  background-color: transparent !important;
  color: transparent !important;
  z-index: -999 !important;
  transform: translateY(-200%);
}

// 兩個 a 連結一個是提示一個是 Froala Logo
// 如果是用參考文章的 position: absolute ，會有跑版問題
```


### 前端顯示編輯器內容

須載入插件的 css，檔名: froala_style.css
路徑: node_modules/froala-editor/css/third_party/

```htmlmixed=
/* 須加上 class 名稱 */
<div class="fr-view">
    <div v-html="content" />
</div>
```