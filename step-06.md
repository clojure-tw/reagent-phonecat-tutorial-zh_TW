# 圖片，連結與屬性

**[Browse code](https://github.com/vvvvalvalval/reagent-phonecat-tutorial/blob/step-6/src/cljs/reagent_phonecat_tutorial/core.cljs#L1) - [Diff](https://github.com/vvvvalvalval/reagent-phonecat-tutorial/compare/step-5...step-6#diff-0bf18c482292a447479e4cfbf8a64631) - [Live demo](http://reagent-phonecat-tutorial.s3-website-us-east-1.amazonaws.com/step-06/)**

開始之前請先執行 git checkout step-6。

***

等會兒會將 app 添加圖片，連結與 CSS 樣式。

大部分的修改都是在 `phone-item` 元件中：

```clojure
(defn <phone-item> "An phone item component"
  [{:keys [name snippet id imageUrl] :as phone}]
  (let [phone-page-href (str "#/phones/" id)]
    [:li {:class "thumbnail"}
     [:a.thumb {:href phone-page-href} [:img {:src imageUrl}]]
     [:a {:href phone-page-href} name]
     [:p snippet]]))
```

- 之前已經看到過，Reagent 允許以 Clojure maps 的方式來表達 HTML 屬性。
- 可以用簡寫表示法來表達 CSS 類別，例如：`[:div.my-class]`。
- 指向每一個手機詳細資料的連結目前還不存在。

## 總結

這個步驟很小，你可以把它想像成跳水前的深呼吸。在**[下個步驟](https://github.com/clojure-tw/reagent-phonecat-tutorial-zh_TW/blob/master/step-07.md)**，我們會學到_路由_。